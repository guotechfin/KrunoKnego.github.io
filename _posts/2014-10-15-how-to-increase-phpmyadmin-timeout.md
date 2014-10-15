---
layout: post
title: "How to increase PHPMyAdmin timeout"
category: 
date: " 2014-10-15 16:21:15"
tags: []
---

Let's say you're importing a large database in your PHPMyAdmin you already increased
the maximum file limit but now you've got a timeout exception.

To fix that you first need to edit field ExecTimeLimit.

<code>$cfg['ExecTimeLimit'] = 300;</code>

and set it to 0

<code>$cfg['ExecTimeLimit'] = 0;</code>

I've first stumbled across this file

![Donotedit]({{ site.url }}/assets/donotedit.jpg){: .center-image}

On my system that file was located on

<code>/usr/share/phpmyadmin/libraries/config.default.php</code>

Obviously you don't edit that file, you'll find your config.inc.php file.
But I found it useful to find all available options of PHPMyAdmin.

config.inc.php file on my system was located on

<code>/etc/phpmyadmin/config.inc.php</code>

And append to the contents of that file the aforementioned line.

<code>$cfg['ExecTimeLimit'] = 0;</code>