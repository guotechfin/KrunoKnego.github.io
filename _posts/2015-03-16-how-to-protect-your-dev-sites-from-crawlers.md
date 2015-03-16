---
layout: post
title: "How to protect your DEV sites from crawlers"
category: 
date: " 2015-03-16 18:15:07"
tags: [htaccess]
---

In this day and time it's important to protect your data. You see all the time that XY company had a security breach
and countless of valuable data was stolen. Now that topic is broad because there are multiple ways a
security breach could have occurred. Instead I'll focus on protecting your development sites not from hackers
but from crawlers like google, bing etc.

Before we even begin with explanation how one would manage to do that let's first see why would you do it in the first
place. One reason could be you've signed NDA with your client and exposing the data would breach your contract.
Unfortunately your development server is online and was crawled by let's say google bots and now you have a problem.
So let's see how could one fix that.

I'm mostly using LAMP stack so I'll give you a guide how to create a basic username/password protection for your
website using apache.

First go to your <code>public_html</code> folder. Open your .htaccess file. If you don't have it then create one. If
you're using shell you could accomplish that like this <code>touch .htaccess</code>.
Now make sure your .htaccess file contains the following lines:

    AuthType Basic
    AuthName "My Password Protected Site"
    AuthUserFile /path/to/your/.htpasswd
    Require valid-user

There's one more step before your site will be password protected and that is creating .htpasswd file.
Same as for .htaccess if you don't have it and you're using shell create it using <code>touch</code>.
Just make sure the path to your .htpasswd matches to the one in .htaccess.

Before proceeding to editing its contents first you need to create password. The password needs to be in MD5 hash
format. I know MD5 is not very safe and can be broken but we're not protecting our site from hackers but from crawlers.
To get MD5 hash of your password you can use [md5hashgenerator](http://www.md5hashgenerator.com/ md5hashgenerator).

Now fill contents of your .htpasswd so it looks like this:

    kruno:b58c2004d062ece3951197da02b5649a

The first value is username and the second value is your password in MD5 hash format.

So this concludes this short tutorial and your site will be now protected from crawlers.
If you were to open your site you'd be greeted with this screen:

![htpasswd]({{ site.url }}/assets/htpasswd.png )

After entering the correct credentials that prompt won't pop out anymore and you can easily test if your development
site is working as intended.

## Read more

* [apache auth](http://httpd.apache.org/docs/2.2/howto/auth.html "apache auth")
