---
layout: post
title: Using Jekyll on Cloud9
date: 2018-12-29 01:33 +0000
---
Creating a website with Jekyll on Cloud9:

{% highlight ruby %}
jekyll --version
bundle show
bundle show minima

export SITE_NAME=blog
export THEME=minima
export HOME=/home/ec2-user

cd ~/environment			# run the command below to make sure you are in the home directory.
gem install jekyll bundler 	# Install Jekyll by running the command below.
ls -al $HOME/.bundle		# confirm bundle directory created
jekyll new $SITE_NAME		# Create a new website 
ls -al $HOME/environment/$SITE_NAME
cd $SITE_NAME				# Change your working directory
{% endhighlight %}

