---
layout: post
title:  "Setting Up Rails With Postgres on Fedora"
date:   2011-10-28 10:51
categories: Programming Rails Postgres Fedora
---

I always forget the complicated setup to get postgres running.
Here's a guide for setting up postgres on Fedora and running a rails app on it.

First install all of our dependencies that we'll need and configure the postgres service.

{% highlight bash %}
% sudo yum install -y postgres postgres-server postgres-devel
% sudo service postgres initdb
% sudo service postgres start
{% endhighlight %}

Now setup a role for yourself. In this example, my username is "et", call it
whatever your login is.

{% highlight bash %}
% sudo -u postgres createuser --pwprompt --encrypted et
could not change directory to "/home/et/code"    # ignore this
Enter password for new role:
Enter it again:
Shall the new role be a superuser? (y/n) n
Shall the new role be allowed to create databases? (y/n) y
Shall the new role be allowed to create more new roles? (y/n) n
{% endhighlight %}

Finally generate your new rails app, adjusting your `config/database.yml` to
match the username and password your created earlier
{% highlight bash %}
% rails new foo -dpostgresql
% vim config/database.yml
% rake db:create
% rails server
=> Booting WEBrick
=> Rails 3.1.1 application starting in development on http://0.0.0.0:3000
=> Call with -d to detach
=> Ctrl-C to shutdown server
[2011-10-28 12:20:07] INFO  WEBrick 1.3.1
[2011-10-28 12:20:07] INFO  ruby 1.9.2 (2011-07-09) [x86_64-linux]
[2011-10-28 12:20:07] INFO  WEBrick::HTTPServer#start: pid=2657 port=3000
{% endhighlight %}


Optionally, tell Fedora to start automatically on a reboot:
{% highlight bash %}
sudo chkconfig postgres on
{% endhighlight %}

Tested on Fedora 15.
