---
layout: post
title:  "JQuery Validation for Email"
date:   2020-07-11 12:00:00 +0200
categories: JQuery Validation Javascript
---
Sometimes the default validation options that you use in JQuery Validation Plugin do not have all the bells and Whistles. I have found that using the out-of-box templates will cover most of the rules you want. But in this case I have found that you may need to make sure it will require a .com at the end of the email. Here is the custom Regex I use for my email fields:

{% highlight javascript %}
/[a-z0-9!#$%&'*+\/=?\^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+\/=?\^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/
{% endhighlight %}

So this will expect the email to look like this:

{% highlight csharp %}
{name}@{Service}.{Domain} 
{% endhighlight %}

But also you would have to apply as a custom rule in your your validation rule step, as listed below:

{% highlight javascript %}
$.validator.addMethod(
  "customEmail",
  function (value, element) {
    return this.optional(element) ||
      /[a-z0-9!#$%&'*+\/=?\^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+\/=?
      \^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+
      [a-z0-9](?:[a-z0-9-]*[a-z0-9])?/.test(value);
  },
  "A valid contact email address is required"
);
{% endhighlight %}

Then in your '''options.rules''' section add this:

{% highlight javascript %}
options.rules = {
  fieldName: {
    required: true,
    customEmail: "A valid contact email is required"
  }
};
{% endhighlight %}

This showcases creating a custom rule for a possible out of box feature not in JQuery Validation Framework.