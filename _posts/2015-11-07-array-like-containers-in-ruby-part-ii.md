---
layout: post
title: Array Like Containers In Ruby - Part II
date: 2015-11-07 15:45
categories: ruby
---

While using the previous solution to solve a problem I've been working on, I started to realize that it wasn't necessarily the best fit. I was using the containers to build tree-like structure which contained calculations, and needed to convert the end result, the calculated tree, into JSON. I shared my solution with a colleague and, although I think they were impressed, they asked me the following question:

> Why don't you just use a Hash?

It's easy for us programmers to over engineer solutions. We want to build new stuff, try out new things, abstract things away. But know your standard libraries and look for the simplest solution first - it may well be the right one :)

{% highlight ruby %}
    {key: 'Foo', store: [1,2,3, ...]}
{% endhighlight %}  
