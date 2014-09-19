---
layout: post
title: "Quick Tip: Internal Server Error"
category: 
date: " 2014-09-19 21:53:42"
tags: []
---

![500]({{ site.url }}/assets/500.jpg){: .center-image}

So this will be a quick tip.

I was deploying my Symfony2 application and for some reason I kept getting Internal Server Error.
I was both in production and development mode and I received the same error screen.

I had some similar problem before. The thing is the problem didn't reach Symfony2 application so I had to check PHP's error log.

But this time the PHP's error log was empty and after some fiddling I checked Apache's error log and there I found something
interesting (You can find Apache error log at /usr/local/apache/logs/error_log).

"Premature end of script headers: app.php"

Now if you were to google solution for that problem you would most likely come up with

This error occurs because the server is expecting a complete set of HTTP headers (one or more followed by a blank line), and it doesn’t get them. This can be caused by several things:
1. Upgrading or downgrading to a different version of PHP can leave residual options in the httpd.conf.
Check the current version of PHP using php -v on the command line and search for any lines mentioning another version in the httpd.conf. If you find them, comment them out, distill the httpd.conf and restart apache.

2. The RLimitCPU and RLimitMEM directives in the httpd.conf may also be responsible for the error if a script
was killed due to a resource limit.

3. A configuration problem in suEXEC, mod_perl, or another third party module can often interfere with
the execution of scripts and cause the error. If these are the cause, additional information
relating to specifics will be found in the apache error_log.

4. If suphp’s log reaches 2GB in size or larger you may see the premature end of scripts
headers error. See what the log contains and either gzip it or null it. Restart apache and then deal
with any issues that the suphp log brought to light. The suphp log is located at: /usr/local/apache/logs/suphp_log

5. The script’s permissions may also cause this error. CGI scripts can only access resources allowed for
the User and Group specified in the httpd.conf. In this case, the error may simply be pointing out
that an unauthorized user is attempting to access a script

Taken from <a href="http://www.liquidweb.com/kb/apache-error-premature-end-of-script-headers/" target="_blank">http://www.liquidweb.com/kb/apache-error-premature-end-of-script-headers/</a>

In my case the problem was regarding permissions.
After I've set 755 permissions to my home directory everything worked flawlessly.
