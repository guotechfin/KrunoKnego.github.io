---
layout: post
title: "Increase upload size of your PHPMyAdmin"
category: 
date: " 2014-10-02 16:32:03"
tags: [phpmyadmin]
---

<h2>Problem</h2>

Let's say you've been trying to import an *.sql file to your PHPMyAdmin.
And let's say that same file has size of about 400MB. You soon notice
that you can't import it because you have limit of 8MB.
Compressing the file won't help you because the file is too big to be reduced to less
then 8MB.

<h2>Solution</h2>

To solve this problem I decided to go the obvious route and that is increase the
upload size. So I looked up where my php.ini file is located, updated
<code>upload_max_filesize</code> and <code>post_max_size</code> fields to 512MB, restart
the apache2 server using command <code>sudo service apache2 restart</code> and I thought that would fix my
problem.

Unfortunately I was still getting that my upload size is only 8MB. So I asked my dear friend Google and it took
me a while until I figured it out. It seems there is also extra.ini file where you can add some additional configuration
for your php.ini file.

In that file fields <code>upload_max_filesize</code> and <code>post_max_size</code> where set to 8MB so that my updates
on php.ini file were overwritten. After increasing them to 512MB as well everything was working fine.

<h2>Conclusion</h2>

My extra.ini file was located at <code>/etc/php5/apache2/conf.d/extra.ini</code> if you don't know where yours is
you might give it try searching with <code>find</code> command like this:

<code>cd /</code>
<br>
<code>sudo find -name extra.ini</code>
