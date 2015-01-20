---
layout: post
title: "Capifony Multistage Extension"
category: 
date: " 2015-01-14 08:59:10"
tags: [capifony, deployment]
---

## Introduction

In this blog post I'll show you small yet powerful capifony's multistage extension.
Before I found out about this extension I was continuously deploying to development and production server by
changing contents of deploy.rb file - this is the main configuration file of capifony.
By using this extension you'll have one command for development another one for production server and you won't need
to worry anymore if you've entered correct information for your server.

## Installation

First replace contents of your deploy.rb with this code.

{% highlight ruby %}
set :stages,        %w(prod dev)
set :default_stage, "dev"
set :stage_dir,     "app/config"
require 'capistrano/ext/multistage'
{% endhighlight %}

Next create prod.rb file in app/config. Make sure its contents are like your usual deploy.rb file and fill it with
server information for production server. Afterwards do the same for dev.rb but use the information for you
development server.

If you have multiple staging servers you can just add them <code>set :stages,        %w(prod dev dev1 dev2 dev3)</code>.
Don't forget to create configuration files for each one of them.

## Usage

To deploy on production: <code>cap prod deploy</code>

To deploy on development: <code>cap dev deploy</code> or <code>cap deploy</code>

## Conclusion

By using multistage extension we save ourselves a lot of time as we don't have to change the information manually.
Next time I'll definitely be paying more attention to documentation and cookbooks of software products I regularly use.


## Read more

* [using-the-multistage-extension](http://capifony.org/cookbook/using-the-multistage-extension.html)
