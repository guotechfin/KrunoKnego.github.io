---
layout: post
title: "SCP   Secure Copy"
category: 
date: " 2014-10-11 15:05:45"
tags: []
---

Today I had a problem. I wanted to copy a file between my host computer and the server but without using
filezilla or any other FTP client. So I googled a bit for a solution that would enable me to copy/paste the
file without ever leaving my terminal.

The solution is called SCP. SCP stands for secure copy. With it you can move files from your server to
your host computer and vice versa.

So let's say you want to upload splash.png file to your server from your host.

You'd simply write

<code>scp splash.png remote_username@remote_serve_ip:/where/you/want/to/put/splash.png</code>

If you wanted to retrieve the file you'd do it like this

<code>scp remote_username@remote_serve_ip:/where/you/want/to/put/splash.png ./</code>

When you're downloading a file from your remote server just remember that the place where you're downloading it
to has to be a directory. So <code>./</code> couldn't be replace with let's say <code>./splash.png</code>

Also if your server is using a specific port you can specify it with "P" flag.

<code>scp -P your_port_number ... </code>




