---
layout: post
title: "Git bisect"
category: 
date: " 2015-07-14 20:38:23"
tags: [git]
---

You have a big project and it has 1000+ commits. A customer of yours reports you that there is a bug. You know that bug wasn't there before but you don't know exactly at what commit.

One option would be to go over commits one by one and try to find where it was working. Then you can compare that commit with the current one and see the difference. Unfortunately that will take a really long time. If only there was a quicker way to find the problematic commit. Enter git bisect.

Bisect is a git command that relies on binary search algorithm. Binary search algorithm works only on sorted arrays. You pick a middle element then declare it bigger or lower then the element you're searching for. Afterwards you take the half of array in which the element is and repeat the process until you find it.

For example if you wanted to find number 6 in [0,1,2,3,4,5,6]. First you'll take the middle element and see if it's bigger or lower then 6. Middle element is 3 and it is lower. We take now take [4,5,6] as our array to search for number 6.
Middle element is 5 and it is lower then 6. Now finally we take [6] as our array and we see there's our number 6. To read more about binary search check the read more part.

In a similar way the bisect command works except we don't declare commits bigger or lower, we declare them good or bad. Before we start with our bisection we have to have an array where we'll perform bisection.

So for example lets declare our latest commit as bad because that's when things didn't work.

<code>git bisect bad</code>

Now let's find a commit in history where things worked. Let's say 100 commits ago everything was working fine.

<code>git checkout HEAD~100</code>

After git checkout command we indeed see that our software is working as expected. So we declare that commit as good.

<code>git bisect good</code>

Now we can finally start with bisection.

<code>git bisect start</code>

You'll be presented with commits. One after another. After each commit you check how your system behave and if it's good (<code>git bisect good</code>) and if it's bad (<code>git bisect bad</code>)

And after some time declaring commits good or bad you'll be presented with a commit that's causing the issue.

## Read More

* [git-bisect](http://git-scm.com/docs/git-bisect "git bisect")
* [bisection-method](https://en.wikipedia.org/wiki/Bisection_method "bisection method")
* [binary-algorithm](https://en.wikipedia.org/wiki/Binary_search_algorithm "binary algorithm")




