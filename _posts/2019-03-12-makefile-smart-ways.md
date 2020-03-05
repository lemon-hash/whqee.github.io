---
layout: post
title: Makefile技巧积累
date: 2019-03-12 20:00:00 +800
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img:  # Add image post (optional)
fig-caption: # Add figcaption (optional)
categories: [Blogging]
tags: [Makefile,嵌入式] # add tag
---

## wildcard递归列出目标
{% highlight bash %}
# Make does not offer a recursive wildcard function, so here's one:
rwildcard=$(wildcard $1$2) $(foreach d,$(wildcard $1*),$(call rwildcard,$d/,$2))

# How to recursively find all files with the same name in a given folder
ALL_MAIN_C := $(call rwildcard,,main.c)

# How to recursively find all files that match a pattern
ALL_SOURCE_C := $(call rwildcard,,*.c)
all:
	echo $(ALL_MAIN_C)
	echo $(ALL_SOURCE_C)
{% endhighlight %}
