---
layout: post
title: "How to use Code Sniffer together with PHPStorm"
category: 
date: " 2015-06-10 18:45:43"
tags: [phpstorm]
---

What is code sniffer? From the name itself you might have an idea or two. It's a PHP application that analyses your code and warns you if you're not following some standards like PSR-2 for example.

If you were to take a look at their github project site there are detailed installation instructions. Code Sniffer consists of two PHP files, phpcs and phpcbf.

The first one just analyses your code and the second one fixes the code for you. Since I don't like the idea of an application fixing the code for me I'll just show you how I've set up the phpcs part together with PHPStorm.

There are several ways you can install phpcs. I've chosen the downloading .phar files via curl way.

<code>curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar</code>

Move that phpcs.phar somewhere where it will be easy to access later on.
I've moved it to <code>/usr/bin</code>.

<code>mv phpcs.phar /usr/bin/phpcs</code>

So now you got CodeSniffer set up. What's missing now is its connection with PHPStorm.

Go to <code>PHPStorm -> Preferences -> Languages & Frameworks -> PHP -> Code Sniffer</code>.

Click on the three dots next to the <code>Local</code> and enter your phpcs path. In my case that was <code>/usr/bin/phpcs</code>. Click on Validate and hopefully you'll get green message.

Now you have to choose by what standards you want to abide. You go to <code>PHPStorm -> Preferences -> Editor -> Inspections</code> select <code>PHP Code Sniffer Validation</code> and chose your code standard.

That's it. Now when you write your code you'll see if you're following the standard. If you aren't, you'll see a message that will tell you what to change to fix it.

