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

Now, a pretty simple way to do this would be, on each update of our game loop,
to add another character from our dialogue string to the string that's actually
getting printed, until we've added all of the characters.

The end result looks pretty good, but that's not a great solution because it
requires tons of string concatenation. In Lua, strings are immutable, so every
time you add a new character onto our printed string, a new string is created.
That means that we're creating a new string that may need to be garbage
collected later for every single letter of dialogue or sign text.

It would be a much more efficient use of memory if we could just add a new
character to the pre-existing string. We can accomplish something similar to
this if we store our string to print as an array of characters instead.

{% highlight lua %}
-- If enough time has passed, and there are more characters to add,
-- add another character to the 'string to print' character array
local printLength = table.getn(textbox.stringToPrint)
if printLength < string.len(textbox.dialogue) and e.hud.timer < 0 then
    local next = table.getn(textbox.stringToPrint) + 1
    table.insert(textbox.stringToPrint, textbox.dialogue:sub(next, next))
    -- Reset the timer
    e.hud.timer = e.hud.typeTimerMax
else
    e.hud.timer = e.hud.timer - dt
end
{% endhighlight %}

However, the problem with this is that the love.graphics API doesn't provide
a mechanism for printing arrays of characters. We'll have to write our own
solution to print out each character in the right position instead.

{% highlight lua %}
love.graphics.setFont(textbox.font)
for i, char in ipairs(textbox.stringToPrint) do
    local offset = i * textbox.fontWidth
    love.graphics.print(char, textbox.x + offset, textbox.y)
end
{% endhighlight %}

Let's try this implementation out and see what happens.

![Animated GIF with awkwardly spaced text](/robrtsql/img/weird-spacing.gif)

Something seems off about the spacing. The font I'm using is [Determination Mono](https://www.behance.net/gallery/31268855/Determination-Better-Undertale-Font)).
Even though it's monospaced, it doesn't seem like indexing to the position
using a fixed character width is a good solution.
