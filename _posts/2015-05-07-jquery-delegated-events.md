---
layout: post
title: "Jquery delegated events"
category: 
date: " 2015-05-07 11:07:37"
tags: [jquery]
---

## What are jquery delegated events?

Jquery delegated events are events that are attached to a parent element.
Those events will be delegated from the parent element to specified child elements.
It doesn't matter whether these children exists now or in future. Once they're created they will properly fire off 
any event listener you wanted to bind on them.

## When & how to use?

Use delegated events when you're working with dynamic elements that need to have certain event listeners.

Let's take a look on a concrete example. Let's say you're working on a table with dynamic content. That content needs
to fire off click event. You noticed that using the standard jquery's <span>.on</span> function doesn't yield results
you wanted. Later on you found a solution that works. It looks something like this:

{% highlight javascript %}
    $( "#my-table" ).on( "click", ".my-content", function(e) {
        console.log($(this));
    });
{% endhighlight %}

Now let's examine this code. We selected the table <span>$("#my-table")</span> and added click element.
The important part lies in the number parameters of the <span>.on</span> function. It isn't two like usual but in fact 
there are three parameters. That's what differentiates a regular click event from a delegated one.
The <span>#my-table</span> element will delegate the click event to all <span>.my-content</span> elements.

## Read More

While reading this examine carefully event propagation also known as event bubbling. 

* [event-delegation](https://learn.jquery.com/events/event-delegation/ "event delegation")

