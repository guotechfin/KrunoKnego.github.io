---
layout: post
title: "How to configure travis for jekyll github page"
category: 
date: " 2015-03-07 10:57:51"
tags: [jekyll, github_pages, travis]
---

## Why Travis

Travis is a service that's used for continuous integration. Each time you push something
travis can ran it for you on their servers using tools you specified and give you information
if build is passing or failing.

The only downside with travis is that it is a paid solution. Fortunately enough that doesn't apply
for open-source projects hosted on github. So let's take advantage of it.

## Jekyll and Travis

First time I decided to integrate travis with jekyll I encountered problems and my build was
constantly failing. In the end I managed to set it up so here's a short tutorial
how to configure everything so you don't have to waste your time like I did.

## Configuration

First create your <code>.travis.yml</code> file.

    #.travis.yml
    language: ruby
    rvm:
    - 2.1

    # branch whitelist
    branches:
      only:
      - master

    before_script:
     - chmod +x cibuild.sh

    script: ./cibuild.sh

    env:
      global:
      - NOKOGIRI_USE_SYSTEM_LIBRARIES=true # speeds up installation of html-proofer“


You can leave out the branches part if you only have one branch but if you have multiple ones then
it might be a good idea to leave it because it will speed up your testing process.

<code>chmod +x cibuild.sh</code> just converts your cibuild.sh script to be executable.
I suspect it would work if you manually changed your cibuild.sh to executable and pushed it to github.
Try it out and comment if it works for you.


When you push your commit to github travis will look first into this <code>.travis.yml</code> file
and do it exactly like you specified it. This configuration, apart from doing other things will execute
cibuild.sh script. So now let's create that script.

{% highlight bash %}
#!/usr/bin/env bash
set -e # halt script on error

bundle exec jekyll build
bundle exec htmlproof ./_site
{% endhighlight %}

This cibuild.sh will build your jekyll site and afterwards execute htmlproofer on _site directory.
Htmlproofer will check your whole site and it will notify you if you're missing title in your hrefs.
It will also tell you if you have img without alt tag and so on. In short it will check if you HTML is valid.

The downside is that it will also give you errors for hrefs with only #. If that's not a mistake and you
want to keep it like that just add <code>data-proofer-ignore</code> so it looks like this:

    <a href="#" data-proofer-ignore>

You can add <code>data-proofer-ignore</code> to other parts of code as well which htmlproofer reports are
invalid but in fact are not.

The last important part of your configuration for travis is Gemfile.
Make sure that you have included everything you use on your site into your Gemfile.

Mine for example looks like this:

    source "http://production.cf.rubygems.org/"

    gem "rake", "~> 10.1.1"
    gem "github-pages", "~> 33"
    gem "html-proofer"
    gem "jemoji"
    gem "jekyll-mentions"
    gem "jekyll-redirect-from"
    gem "jekyll-sitemap"


## Travis

Now after you've finished with configuring your github page for travis go to [travis-ci.org](https://travis-ci.org/ "travis")
and sign in with your github account. Finally turn on travis for your github pages repository.

## Conclusion

Congratulations. You finished with the tutorial and your each push will create a travis instance which will
check if your HTML code is valid. Once you've successfully completed this tutorial you might want to include some
other tests.







