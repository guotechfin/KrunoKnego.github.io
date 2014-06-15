---
layout: post
title:  "Doctrine2 optimization tips"
date:   2014-05-15 23:06:00
categories: symfony2 doctrine2 optimization
---

![Doctrine2]({{ site.url }}/assets/doctrine2.png){: .center-image}

If you ever found yourself asking why your doctrine2 is suddenly going slow or got "Killed" message in your terminal
when you used doctrine then this post is for you.

I've been assigned with a task to parse large amount of data using doctrine2. Soon I found out all about limitations of
ORMs. I frequently received "Killed" messages, parsing data got increasingly slower and I was about to ditch ORM
for that project completely.

But after fiddling a little bit more with doctrine2 I found that you can still use doctrine if you follow these few rules.

## Rule 1. Disable SQL-logging

 You received "Killed" messages because of memory leaks. Linux has OOMKiller that when a process uses too much
 resources it simply kills the said process.
 Now if you're testing in development mode chances are that you haven't disabled SQL logging. To do that you simply
 include this piece of code in your project:

{% highlight php5 %}
    $this->em->getConnection()->getConfiguration()->setSQLLogger(null);
{% endhighlight %}

If you're using Symfony2 and you wrote a command. Then you can ignore the piece of code above and simly add
--env=prod --no-debug to your command.


{% highlight bash %}
    php app/console name-of-your-command --env=prod --no-debug
{% endhighlight %}


## Rule 2. Clear your entity manager frequently

Keep your entity manager clean. If you're dealing with a lots of data doctrine2 will keep all the objects
in the entity manager. So I advise you to clean it regularly using command

{% highlight php5 %}
    $this->em->clear();
{% endhighlight %}

I found that it works best for me when I clear entity manager after I persisted and flushed about 50 objects.
You can play with this number and find what works you.

One of the problems I encountered while clearing the entity manager is that you can't go over your foreach loop
with data you pulled with doctrine2 afterwords.

So to combat that problem you can either pull your data with offset or you could just
go layer underneath doctrine2 ORM and use DBAL (Database Abstraction Layer).

I usually get it via symfony2 service

{% highlight php5 %}
    $connection = $this->get('database_connection');
    $connection->fetchAll('SELECT * FROM table');
{% endhighlight %}



If you follow these rules your doctrine2 should behave much smoothly but if you're still experiencing
performance issues I suggest you use solely DBAL.



