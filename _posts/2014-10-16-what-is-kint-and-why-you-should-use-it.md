---
layout: post
title: "What is Kint and why you should use it"
category: 
date: " 2014-10-16 13:09:33"
tags: []
---

So you're a PHP developer, huh!? Well then you must be using <code>var_dump</code> or
<code>print_r</code>. Well I'm going to show you that using them should go in history because there's new kid in
the block and his name is Kint.

Kint is nothing but a debugging tool for PHP based projects. I have experience in using Kint  only in Symfony2 projects.
In Symfony2 projects you just add <code>"raveren/kint": "1.0.*@dev"</code> to your composer.json and update it.

You don't have to worry about including Kint in any of the files because composer will autoload it.

To debug a variable you simply write <code>dd($someVar)</code>

![Kint_1]({{ site.url }}/assets/kint1.jpg ){: .center-image}

You'll get a nice visual representation of your variable. But you might say hey I can do that with <code>var_dump</code>
as well. It's not pretty but I get things done.

![Vardump]({{ site.url }}/assets/vardump.jpg ){: .center-image}

Well wrong, have you ever used <code>var_dump</code> on an object? You're still waiting for it load?

Kint doesn't have such problem. Let's see what happens when you debug an object using Kint.

![Kint_2]({{ site.url }}/assets/kint2.jpg ){: .center-image}

Kint also comes with nice set of features. If you prepend dd with ! you'll get already expanded data

<code>!dd($someVar)</code>

Also you don't have to worry about using <code>die();</code> function because your code will automatically be halted
when you use kint with two d's. If you want to prevent that option you should use only one d. It goes like this

<code>d($someVar)</code>

To read more about kint check their official website <a title="Kint" href="http://raveren.github.io/kint/">kint</a>
