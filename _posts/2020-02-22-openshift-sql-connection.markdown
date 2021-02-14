---
layout: post
title:  "Setting up a SQL connection string in Red Hat's Openshift"
date:   2020-02-22 12:05:13 +0200
categories: SQL Openshift Kubernetes
---
Sometimes its hard to find out the best and safest way to connect a Linux Container to a SQL Server Database. All it requires is a local SQL account on the server and I little configuration in Openshift.

First off you will have to think about how to dynamically build the connection string in code. I would use the .Net Standard Class: SqlConnectionStringBuilder. This class cuts all the important pieces into properties you can define in the class.

{% highlight csharp %}
// Build DB ConnectionString
var connString = new SqlConnectionStringBuilder(
  Configuration.GetConnectionString("connString"));
{% endhighlight %}

After you add the string builder in startup, you can assign Environment Variables to each property to populate by a certain environment.

{% highlight csharp %}
connString.DataSource = Environment.GetEnvironmentVariable("DATASOURCE") ?? string.Empty;
connString.InitialCatalog = Environment.GetEnvironmentVariable("INITIALCATALOG") ?? string.Empty;
connString.UserID = Environment.GetEnvironmentVariable("UserID") ?? string.Empty;
connString.Password = Environment.GetEnvironmentVariable("PASSWORD") ?? string.Empty;
{% endhighlight %}

Once you set the environment variables to the properties. You will have to define the password as a secret in Openshift. And you would reference the secret under the environment variable section in your openshift yaml deployment template (As seen below).

{% highlight yaml %}
spec:
  containers:
  - name: Sample Application
  env:
  - name: ASPNETCORE_ENVIRONMENT
    value: Development
  - name: DATASOURCE
    value: (LocalDB)\MSSQLLocalDB
  - name: INITIALCATALOG
    value: Sample-Database
  - name: USERID
    valueFrom:
      secretKeyRef:
        key: username
        name: svc-test-dev-sql
  - name: PASSWORD
    valueFrom:
      secretKeyRef:
        key: password
        name: svc-test-dev-sql
{% endhighlight %}

This provides a simple way to connect the startup application in .Net Core to an instance of a SQL Server.
