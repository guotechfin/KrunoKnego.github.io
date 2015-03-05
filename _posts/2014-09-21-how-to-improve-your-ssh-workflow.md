---
layout: post
title: "How to improve your SSH workflow"
category: 
date: " 2014-09-21 18:05:15"
tags: [linux, ssh]
---

![SSH]({{ site.url }}/assets/ssh.jpg ){: .center-image}

Did you ever need to log on to remote server via SSH? Are you tired of constantly
searching for the username and/or password of your remote server? If your answer to these questions
is yes then read on. You'll definitely find it interesting.

The first thing you need to do is check if you have your private and public keys set up.
If you don't have them set up or you're not sure just write the following command :

{% highlight bash  %}
    ssh-keygen
{% endhighlight %}

It will automatically generate private and public keys for you. The next step is to use command
ssh-copy-id.

If you find yourself using mac machine you have to first install <code>ssh-copy-id</code> or
you will get "Command not found" error.

{% highlight bash  %}
sudo curl https://raw.githubusercontent.com/beautifulcode/ssh-copy-id-for-OSX/master/ssh-copy-id.sh -o /usr/local/bin/ssh-copy-id
sudo chmod +x /usr/local/bin/ssh-copy-id
{% endhighlight %}

Or you could use brew and install it like this:

{% highlight bash  %}
    sudo brew install ssh-copy-id
{% endhighlight %}

The usage of <code>ssh-copy-id</code> goes like this:

{% highlight bash  %}
    ssh-copy-id username@remote-server.com
{% endhighlight %}

This command will copy your public key to the remote server and once you try to connect via
ssh command next time it won't ask you for a password.

Now you can connect to your server without a password and that's great. But it can be a drag to remember
username or port or even domain name so let's dumb it down a little bit more.

First step is to position yourself to .ssh directory

{% highlight bash  %}
cd ~/.ssh
{% endhighlight %}

If you were to type in the "ls" command you'd see that directory is populated with generated private and
public keys. To move to the next step we have to create a config file. There are numerous ways
to do it. You can do it with vim, nano or something else. Use whichever editor you're
most comfortable with.

Fill the config file with following lines:
{% highlight bash  %}
Host myServer
HostName yourserver.com
User yourusername
IdentityFile ~/.ssh/id_rsa
{% endhighlight %}

I propose you fill out the first line with some easy to remember name. In this example I've named it
myServer but that can be anything you want. HostName can be written as yourserver.com or you can
put whole IP address of the server.
IdentifyFile part is used for defining where your generated key is located.

There are also other configuration options. For example if your server's secure port isn't default 22 but some else
 you can just add "Port XXXX" to your config file.

After you've made all the changes now you can simply write:
{% highlight bash  %}
ssh myServer
{% endhighlight %}

And you should be logged in.

So there you have it. This configuration might take you a while to complete the first time but the
benefits it adds are enormous. No more searching where is password for serverA or
username for serverB and so on.

If you're interested in speeding up your ssh connection read more <a href="https://puppetlabs.com/blog/speed-up-ssh-by-reusing-connections" title="puppetlabs">here</a>.

