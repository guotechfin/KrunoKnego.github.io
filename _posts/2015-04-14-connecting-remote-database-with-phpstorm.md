---
layout: post
title: "Connecting remote database with PHPStorm"
category: 
date: " 2015-04-14 23:40:47"
tags: [phpstorm, vagrant]
---

## Introduction

PHPStorm has great feature which allows you to connect to a database within IDE. To check it out you can simply
select Database option from View/"Tool Windows" or you can click on the Database in the top right corner of your
PHPStorm. The great thing about said feature is that you can connect on local databases as well as remote ones.
Remote databases require more configuration. It isn't that difficult you just need to configure SSH tunnel.

## Configuration

1. Open Database
2. Click on the plus icon
3. Choose Data Source
4. Select preferred type of database
5. Fill details of your database
6. Fill details of your SSH connection under SSH/SSL tab - applies only for remote database

## Problem?

That's it. As you can see for yourself the configuration is really simple and straightforward, if you have all required
details. The point of writing this blog post in the first place was that I lost a lot of time and almost gave up.
Each time I filled details and clicked on Synchronize button I got an error.

java.rmi.ConnectException: Connection refused to host: localhost; nested exception

I've thought for a long time that the problem lies in me writing wrong credentials but actually the problem was
somewhere else entirely. Later on I found out that I have to check my hosts file. I had 127.0.0.1 localhost
commented and that's the reason it didn't work. Once I've uncommented it the problem was resolved and everything worked
like a clockwork.

## Conclusion

Sometimes even simple things can be hard but if you aren't easily discouraged you'll eventually resolve your problem.
Hopefully, to some, that resolution might even come from this trivial blog post.
