---
layout: post
title: Basic Jekyll Commands
date: 2020-11-08 17:45 +0000
---
Using basic Jekyll commands:

{% highlight ruby %}
The jekyll program has several commands but the structure is always:
jekyll command [argument] [option] [argument_to_option]
Examples:
    jekyll new site/ --blank
    jekyll serve --config _alternative_config.yml


jekyll help			# Generates help documentation\
jekyll help build	# Shows help, optionally for a given subcommand
jekyll doctor		# Checks for URL conflicts
jekyll clean		# Removes all generated files: destination folder, metadata file, Sass and Jekyll caches.
{% endhighlight %}
