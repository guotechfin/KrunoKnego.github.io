---
layout: post
title: "Jekyll tags for github pages"
category: 
date: " 2015-03-03 20:29:36"
tags: [jekyll, github_pages]
---

## Jekyll!? Jekyll and Hyde?

So first of all let's clarify what jekyll exactly is. Jekyll is a platform that you can use for blogging.
It converts dynamic content into static web pages.

If you would like to host a web page on github you'd soon notice that you can't have dynamic content. That's
exactly where jekyll comes in. It converts dynamic content of your blog into a series of static pages.

To learn more about jekyll and how to configure it you can read at [jekyllrb.com](http://jekyllrb.com/).

## Jekyll and tags

Actually there are plugins that give you functionality of tags but github allows only limited amount of
plugins.

As of writing this blog post these jekyll plugins are supported:

* Jemoji - provides support for emoji within Jekyll posts and pages
* Jekyll-mentions - provides support for @mentions within Jekyll posts and pages
* Jekyll-redirect-from - redirects visitors to an updated URL when Jekyll post or page filenames change.
* Jekyll-sitemap - adds a standards compliant sitemap for your GitHub Pages.

As you can see there is currently no tag plugin supported.

## Solution

So what do we do? Well we build it ourselves.
I've been searching for a solution and soon stumbled upon the following website [tags-and-categories-on-github-pages](http://www.minddust.com/post/tags-and-categories-on-github-pages/)

The solution proposed on that website is good but some steps were omitted.
For it to work you have to enable relative permalinks in your config.

    # _config.yml
    relative_permalinks: true to

I've also simplified the logic for generating tags with their respective links:

{% highlight ruby %}
    {% for post in paginator.posts %}
      <div class="post">
        <a class="post-link" title="{{ post.title }}" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
          <div class="clear-float"></div>
          <div class="date">{{ post.date | date: "%b %-d, %Y" }}</div>
          {% for tag in post.tags %}
            <a class="tag" href="/blog/tag/{{ tag }}/">{{ tag }}</a>
          {% endfor %}
      </div>
    <hr/>
    {% endfor %}
{% endhighlight %}

The important part is

{% highlight ruby %}
  {% for tag in post.tags %}
    <a class="tag" href="/blog/tag/{{ tag }}/">{{ tag }}</a>
  {% endfor %}
{% endhighlight %}

Which is considerably shorter then approach given on [tags-and-categories-on-github-pages](http://www.minddust.com/post/tags-and-categories-on-github-pages/)

## Conclusion

There are more complex solutions where you can have categories but for my needs implementing tags as easily as
possible was enough.


