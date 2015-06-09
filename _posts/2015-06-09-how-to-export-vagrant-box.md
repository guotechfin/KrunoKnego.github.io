---
layout: post
title: "How to export vagrant box"
category: 
date: " 2015-06-09 20:51:37"
tags: [vagrant]
---
Before describing the actual steps for exporting your vagrant box from one machine to another let me give you few examples where it might be useful.

The first one would be you want to get your colleague up to speed and you don't t want him to wait for the whole process of provisioning.

The second one would be even more useful. Let's say you've added few important work-related stuff to your Vagrant Box but still haven't updated the provisioning for it. Then the quick solution would be to export it and your colleague will have the same box as you do.

Now let's get down to the actual exporting process. To accomplish it firstly start up your vagrant machine via <code>vagrant up</code> command. Don't ssh into it.
Now use <code>vagrant package</code> command. This will export the running machine into package.box file. You can later on share that file and people can quickly install the box.

To actually import the box you'd use the following command:

<code>vagrant box add package.box --name vm_name</code>

Make sure that vm_name is the same as the one defined in Vagrantfile. Otherwise it will report that VM is missing and it will try to download it if a VM url is available.

Now when your colleague does the <code>vagrant ssh</code> command he'll have the same environment as you do.


