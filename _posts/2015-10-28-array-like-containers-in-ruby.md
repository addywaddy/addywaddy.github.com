---
layout: post
title: Array Like Containers In Ruby
date: 2015-10-28 12:59
categories: ruby
---

I found myself today wanting to add behaviour to the Array class in ruby, specifically to add a 'key' accessor.

My initial naive attempt was to subclass Array, but there are a number of reasons why you don't want to do that (see [here](http://words.steveklabnik.com/beware-subclassing-ruby-core-classes) for more details).

This is the simplest solution I managed to come up with. It doesn't behave completely like an Array but covers my use case well:

{% highlight ruby %}
class Container
    attr_accessor :key
    include Enumerable
    extend Forwardable
    def_delegators :@ary, :each

    def initialize(ary = [])
      @ary = ary
    end
  end
{% endhighlight %}
