---
layout: post
title:  "How to setup a Serilog OutputTemplate and Console Theme"
date:   2020-09-05  12:00:00 +0200
categories: Serilog .Net Core JSON
---
Sometimes it is hard to read a kestrel console log and find errors or warnings when you are debugging your app.
With Serilog you can set themes for the console and it can color code what the important identifiers can be.
But also you can format the order of the different pieces of information shown on each logline.

{% highlight js %}
"outputTemplate": "[{Timestamp:o} {Level:u4}] {{{RequestId}}} {Message} {NewLine}{Exception}"
{% endhighlight %}

The output template would be an argument that you can use in the ```appsettings.json``` 
and it would be used to show exactly what formats the logline from left to right.
\
[Click here for more info on output templates](https://nblumhardt.com/2014/05/serilog-output-template-formatting-changes)
\
Also within the arguments block, you can set up a couple of different themes.
The one I enjoy is the ```AnsiConsoleTheme``` it showcases the colors red for error and yellow for warning really well.

{% highlight js %}
"theme":"Serilog.Sinks.SystemConsole.Themes.AnsiConsoleTheme::Code, Serilog.Sinks.Console"
{% endhighlight %}
\
[Click here for more info on console themes](https://nblumhardt.com/2017/06/ansi-console)
\
So with both of these settings, you can combine them in a Serilog JSON object to output to a console.

{% highlight js %}
"Serilog": {
  "WriteTo": [
    {
      "Name": "Console",
      "Args": {
        "theme":"Serilog.Sinks.SystemConsole.Themes.AnsiConsoleTheme::Code, Serilog.Sinks.Console",
        "outputTemplate": "[{Timestamp:o} {Level:u4}] {{{RequestId}}} {Message} {NewLine}{Exception}"
      }
    }
  ]
}
{% endhighlight %}

Of course you can adjust the parameters listed in the outputTemplate to show the way you like in the logs.