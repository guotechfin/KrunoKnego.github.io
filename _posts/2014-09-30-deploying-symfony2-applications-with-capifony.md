---
layout: post
title: "Deploying Symfony2 applications with Capifony"
category: 
date: " 2014-09-30 10:55:16"
tags: []
---

<h2>Introduction</h2>

<i>Capistrano is an open source tool for running scripts on multiple servers. It’s primary use is for easily deploying applications. While it was built specifically for deploying Rails apps, it’s pretty simple to customize it to deploy other types of applications.
    capifony is a deployment recipes collection that works with both symfony and Symfony2 applications.</i>
Taken from <a href="http://capifony.org/">capifony.org</a>

<h2>Installation</h2>

In order to install capifony you have to use RubyGems. RubyGems is a package manager for ruby programs.

<code>gem install capifony</code>

On OSX you might need <code>sudo</code>.

<code>sudo gem install capifony</code>

<h2>Deployment</h2>

Go to folder of your Symfony2 application

<code>cd your/path/to/s2_app</code>

Now we have to initialize capifony.

<code>capifony .</code>


<h2>Conclusion</h2>