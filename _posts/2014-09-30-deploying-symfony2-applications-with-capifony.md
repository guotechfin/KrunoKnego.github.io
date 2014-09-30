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

On OSX you might need to use <code>sudo</code>.

<code>sudo gem install capifony</code>

<h2>Configuring deploy.rb</h2>

Go to folder of your Symfony2 application

<code>cd your/path/to/s2_app</code>

Now we have to initialize capifony.

<code>capifony .</code>

<img src="{{ site.url }}/assets/capifony.jpg" alt="Capifony initialized"/>

This will create two files Capfile and deploy.rb. The Capfile is a capifony configuration file and deploy.rb is a file where
you write your server information.

You can ignore Capfile but we have to modify the deploy.rb file.
It can be found in <code>app/config/deploy.rb</code>.
Now if you were to open that file its contents should look something like this.

{% highlight ruby %}
set :application, "set your application name here"
set :domain,      "#{application}.com"
set :deploy_to,   "/var/www/#{domain}"
set :app_path,    "app"

set :repository,  "#{domain}:/var/repos/#{application}.git"
set :scm,         :git
# Or: `accurev`, `bzr`, `cvs`, `darcs`, `subversion`, `mercurial`, `perforce`, or `none`

set :model_manager, "doctrine"
# Or: `propel`

role :web,        domain                         # Your HTTP server, Apache/etc
role :app,        domain, :primary => true       # This may be the same as your `Web` server

set  :keep_releases,  3

# Be more verbose by uncommenting the following line
# logger.level = Logger::MAX_LEVEL
{% endhighlight %}

Replace the contents of deploy.rb with the following code:

{% highlight ruby %}
logger.level = Logger::MAX_LEVEL

set :application, "__1__"
set :domain,      "__2__"
set :deploy_to,   "/home/__3__"
set :user,        "__4__"
set :password, "__5__"

role :web,        domain
role :app,        domain
role :db,         domain, :primary => true
# ssh_options[:port] = "__6__"


set :scm,         :git
set :repository,  "__7__"
set :branch,      "master"
set :deploy_via,  :remote_cache

ssh_options[:forward_agent] = true

set :use_composer,   true
set :update_vendors, true
default_run_options[:pty] = true
set :composer_options,  "--verbose --prefer-dist"

set :dump_assetic_assets, true


set :writable_dirs,     ["app/cache", "app/logs"]
set :webserver_user,    "nobody"
set :permission_method, :acl
set :use_set_permissions, false

set :shared_files,    ["app/config/parameters.yml", "web/.htaccess", __8__]
set :shared_children, ["app/logs"]

set :model_manager, "doctrine"

set :use_sudo,    false

set :keep_releases, 3

after "deploy:update_code" do
capifony_pretty_print "--> Ensuring cache directory permissions"
run "chmod -R 755 /home/__9__/"
capifony_puts_ok
end


task :upload_parameters do
origin_file = "app/config/parameters_prod.yml"
destination_file = latest_release + "/app/config/parameters.yml" # Notice the latest_release

try_sudo "mkdir -p #{File.dirname(destination_file)}"
top.upload(origin_file, destination_file)
end

task :upload_httaccess do
origin_file = "web/.htaccess"
destination_file = latest_release + "/web/.htaccess" # Notice the latest_release

try_sudo "mkdir -p #{File.dirname(destination_file)}"
top.upload(origin_file, destination_file)
end

before "symfony:composer:update", "upload_parameters"
#after "deploy", "upload_httaccess"



after "deploy:update", "deploy:cleanup"
after "deploy", "deploy:set_permissions"
{% endhighlight %}

Replace <code>__1__</code> with your application's name. Example: myApp

Replace <code>__2__</code> with your domain name. Example: myapp.com

If you're unsure what to write on <code>__3__</code> just use <code>ssh</code> to connect to your server and afterwards <code>pwd</code> to
see what's your folder's name.

Replace <code>__4__</code> with your server's username

Replace <code>__5__</code> with your server's password

Replace <code>__6__</code> with your port number. This step is optional and you should fill it out only if you're not using default ssh port.
Also don't forget to remove <code>#</code> to uncomment that part of code.

Replace <code>__7__</code> with your git repository. Example git@bitbucket.org:someRepo.git

<code>__8__</code> you write here your folder's name you want to share. This part will be explained in more detail later.

For <code>__9__</code> see <code>__3__</code>

<h2>Configuring server</h2>

Now after we successfully modified our deploy.rb file we have to prepare our server for capifony.
Use <code>ssh</code> to log on to your server.

You should be /home/__some_name_

Create folders called releases and shared.

<code>mkdir releases</code>
<code>mkdir shared</code>

When application is deployed capifony will clone your git repository in releases folder.
In deploy.rb file there was line <code>set :keep_releases, 3</code> you can change that number to any amount you want.

After your git repository has been clone it will get the latest release and symlink it on to current folder.
Contents of your shared folder will be symlinked to your current folder.

This is excellent when you have let's say user uploaded images. This will enable you to share your images across releases.
So if you were to replace <code>__8__</code> with something like <code>web/images</code> that would symlink your images folder in your
current release web folder.

The shared folder structure should be something like this:

shared/app/config/parameters.yml
shared/web/.htaccess
shared/web/images

<h2>Deployment</h2>

Exit your ssh connection and position yourself to your symfony2 application:

<code>cd your/path/to/s2_app</code>

And now let's deploy our symfony2 application.

<code>cap deploy</code>

Give it few minutes to complete and now you have to do one more final step.
<code>ssh</code> to your server and symlink current/web to your public_html.
If you already have public_html folder you have to first remove it.

<code>rm -rf public_html</code>
<code>ln -s current/web public_html</code>

You will have to do this last part of configuration only the first time.

For future deploys you only need to write <code>cap deploy</code>

<h2>Additional features</h2>

Capifony has lots of additional features to list them all write:
<code>cap -vT</code>

I personally find very useful <code>cap deploy rollback</code> option.

Let's say you deploy your application but there's some bug in code so while you're fixing it you can
just write <code>cap deploy rollback</code> to rever to the previous release.

<h2>Conclusion</h2>

I believe I've demonstrated through this short tutorial just how easy it is to deploy your symfony2 applications.
Sure you might have some trouble configuring all this the first time but in the long run it will pay off.