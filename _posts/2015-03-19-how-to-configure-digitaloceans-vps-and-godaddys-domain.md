---
layout: post
title: "How to configure DigitalOcean's VPS and GoDaddy's domain"
category: 
date: " 2015-03-19 22:03:34"
tags: [digitalocan, godaddy]
---

So if you have bought a VPS on Digital Ocean and domain on GoDaddy and you're wondering how to make them work together
this tutorial is made specifically for you. This short tutorial with picture examples just tries to get you started
as quickly as possible. If you want to understand how it works I suggest you check out the read more section. So let's
get started.

First go to [GoDaddy](http://godaddy.com "Go Daddy") and sign in. Now open your domains and click manage.

![GoDaddy DNS Configuration 1]({{ site.url }}/assets/dns1.png)

![GoDaddy DNS Configuration 2]({{ site.url }}/assets/dns2.png)

Add digital ocean's nameservers. Their nameservers are:

* ns1.digitalocean.com
* ns2.digitalocean.com
* ns3.digitalocean.com

![GoDaddy DNS Configuration 3]({{ site.url }}/assets/dns3.png)

Now head off to [DigitalOcean](https://cloud.digitalocean.com/ "Digital Ocean") sign in and create your droplet if you
haven't already done that. To create your droplet is really simple. It mostly comes down to selection of your preferred
region, operating system and stack which you want to use (LAMP, Wordpress, Docker etc.).

Finally click on DNS and afterwards add domain. Enter URL of your website for example: mysite.com and just choose your
droplet the IP Address part will be automatically filled.

![DigitalOcean DNS Configuration 1]({{ site.url }}/assets/dns4.png)

![DigitalOcean DNS Configuration 2]({{ site.url }}/assets/dns5.png)


## Read more

If you wish to learn more about DNS definitely read a complete series of tutorials on managing DNS
[an-introduction-to-managing-dns](https://www.digitalocean.com/community/tutorial_series/an-introduction-to-managing-dns "An introduction to managing DNS")
There you will learn what A, AAAA, CNAME and other cryptic acronyms mean and you will gain better understanding how
whole communication between GoDaddy and DigitalOcean works.



