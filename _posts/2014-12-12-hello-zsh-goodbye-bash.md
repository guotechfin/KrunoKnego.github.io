---
layout: post
title: "Hello ZSH; Goodbye Bash"
category: 
date: " 2014-12-12 14:39:59"
tags: []
---

##Introduction
I’m a converted mac user. I used to hate Apple products with all my heart because I thought they were pretentious. But seriously. Once you begin using an Apple product you see what’s the hype all about. Put aside the social status and everything else that goes with it. The products are very good and enjoyable to use.

Now I’m proud owner of macbook air. I familiarized myself with the terminal and learned to love it. Macbook computers come pre-installed with archaic bash version and it was kind of dull looking. So I decided to replace it.

Enter ZSH. I initially looked for bash replacement because it was dull looking but ZSH is so much more than that. There are numerous features that will definitely make you think about replacing your good old bash.

##Installation

Installing ZSH on Mac is fairly simple. To make it as painless as possible I suggest you install brew first.
```ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```

Now installing zsh is very easy.
```brew install zsh```
add ```/usr/local/bin/zsh to /etc/shells```

Now finally my suggestion is to install Oh-My-ZSH
You just execute the following link ```curl -L http://install.ohmyz.sh | sh````

If you want ZSH without anything else then just execute

```chsh -s /usr/local/bin/zsh```
To change your shell

## Features

Now let’s see why exactly is ZSH superior to bash.

Let’s say you changed quite a few directories. Now you want to return to a specific one with minimum amount of typing. In ZSH you’d just press
d and then choose number to which directory you want to return.
![History of directories]({{ site.url }}/assets/zsh/directory.gif)

In ZSH you don’t even have to write cd. You can simply write name of directory, press enter and ta-da! You’ve successfully changed directory.
![Change directory without cd]({{ site.url }}/assets/zsh/cd.gif)

It also comes with powerful auto-complete feature. You can use arrow keys to select which directory you want to switch to.
![Auto-complete ZSH]({{ site.url }}/assets/zsh/cd_completion.gif)

Just look how powerful ZSH is. If the first few letters are unique and you press TAB it will automatically expand to full name of the directory.
![Auto-complete ZSH tab expand]({{ site.url }}/assets/zsh/cd_completion_2.gif)

This is globbing. Here is **/ pattern used which tells zsh to go any depth till it matches directory ansible.
![ZSH Globbing]({{ site.url }}/assets/zsh/globbing.gif)

## Wrap-up

I’m sure I missed some great features of ZSH but these are the ones I’m currently aware of.
To make your ZSH even more powerful enable vi mode ```bindkey -v```

## Read more

* [why-zsh-is-cooler-than-your-shell](http://www.slideshare.net/jaguardesignstudio/why-zsh-is-cooler-than-your-shell-16194692)
* [globbing-wildcard-characters-with-zsh](http://www.techrepublic.com/article/globbing-wildcard-characters-with-zsh/)
