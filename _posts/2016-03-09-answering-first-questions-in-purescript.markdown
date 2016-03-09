---
published: false
title: Answering first questions in PureScript
layout: post
---
# Day 2

Today I started to read for the 3rd time
[Purescript by Example](https://leanpub.com/purescript/read).
First time with proper idea what I want to write :)

It should be helpful in future learning.

Following [chapter 3](https://leanpub.com/purescript/read#leanpub-auto-functions-and-records)
we can find that **indentation** matters. That's a good finding!

And yes - we have at least one winner!
In [3.13](https://leanpub.com/purescript/read#leanpub-auto-infix-function-application)
there's a nice explenation what does *$* mean!
It it connected with *curring*. It can be used to omit parentheses.
It makes code more readible.
Haskell people are using it all the time so probably it'll be our second nature.

*$* can be readed as: "evalutate on the right of it and apply result to
function on the left side on it".

Working example (after installing
[purescript-lists](https://github.com/purescript/purescript-lists/blob/master/docs/Data/List.md))

Take first element from generated list of integers from 1 to 20
{% highlight haskell %}
head range 1 20 -- it will throw because PureScript interpreter will try to apply `range` to `head` and then 1 to result of it

head $ range 1 20 -- it will succeed because interpreter will first evaluate the range and next it'll take first element
{% endhighlight %}

Reading this chapter and looking at corresponding
[source code](https://github.com/paf31/purescript-book/blob/master/chapter3/src/Data/AddressBook.purs)
we cannot find other answer to yesterday's questions (what does *= do* mean).
Instead we can find some new language constructions:

 - `::` - similar to [Haskell's](https://www.haskell.org/tutorial/goodies.html) type definition.
On the left side we have name, on the right side - types
 - functions takes *always* one argument with [automatic curring](https://leanpub.com/purescript/read#leanpub-auto-curried-functions)
 - `>>>` and `<<<` - [composition](https://en.wikipedia.org/wiki/Function_composition_(computer_science)) operators.
Bread and butter in functional world. One is "from left" and another is "from right".

### Composition

Let's try consider following examples - we want all elements but first from filtered list of integers:

{% highlight haskell %}
tail <<< filter (>10) $ 1..20 -- it will work
filter (>10) >>> tail $ 1..20 -- this line behaves the same
{% endhighlight %}
