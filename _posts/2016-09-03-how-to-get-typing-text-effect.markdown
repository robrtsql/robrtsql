---
layout: post
title:  "How to get typing text effect"
date:   2016-09-03 18:14:31 -0500
categories: love2d gamedev lua
---
I'm working on a game right now, and in this game I want dialogue text from
talking to NPCs or reading signs to appear as if it is being typed. This is
a common effect which you might see in a game like Earthbound.

I am using [LÖVE](https://love2d.org/) for my game because it's an easy-to-use
framework with tons of community-authored libraries.

Let's try this implementation out and see what happens.

![Animated GIF with awkwardly spaced text](/robrtsql/img/weird-spacing.gif)

The spacing seems to be off, right?

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}
