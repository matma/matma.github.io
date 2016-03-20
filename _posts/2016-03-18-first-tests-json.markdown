---
published: true
title: First tests & JSON
layout: post
---
Our bot will be working with JSON data. So we can learn some fancy things today:

## Types

Everything is a type. A strong type. No exception in JSON. GitHub's JSON format
is known. So we can convert string representation to *instance of type*.

In PureScript there're `type`s, `data`s & `record`s. As far as I know. We must
use `data` in case of decoding JSON. Probably future will tell us why :)

First let's define type:

{% highlight haskell %}
data PushEvent = PushEvent {
  ref :: String
}
{% endhighlight %}

Our first test - read & test `ref` property.

## Unit testing

Pulp itself has `pulp test` command which runs tests from `Test`  folder. This
folder can be used as a battlefield as well :)

We need some assertion library - basic
[purescript-assert](https://github.com/purescript/purescript-assert) is enough.

Which points us to JSON.

## Test data

To parse JSON we will use
[purescript-foreign](https://github.com/purescript/purescript-foreign) package.
After installation we can test it using `pulp test`.

But first we should have some data to work with. Fortunately for us GitHub
provides example of
[PushEvent](https://developer.github.com/v3/activity/events/types/#pushevent)
which will be sent by GitHub when Push happened to repository.

Lets download it and save into `Test/push_event.json`.

Now we should read it :) There's a wrapper for Node's FS module -
[purescript-node-fs](https://github.com/purescript-node/purescript-node-fs). We
will use it.

{% highlight haskell %}
getPushEventFromJSON = do
  content <- readTextFile UTF8 "test/push_event.json"
  return $ readJSON content :: F PushEvent
{% endhighlight %}

Simple - *bind* contents of "test/push_event.json" file to *content*. More
about bind in future episodes :)
Next - parse JSON into proper object - PushEvent.

There's however one gotcha - the result is an
[either](https://github.com/purescript/purescript-either) type. We'll need to
handle it differently.

## Assert

Let's write our `main` function:

{% highlight haskell linenos %}
checkParsedMessage p =
  p.ref == "refs/heads/changes"

main = do
  push_event <- getPushEventFromJSON
  case push_event of
    Right (PushEvent p) -> assert' "Wrong value after parsing!" $ checkParsedMessage p
    Left  _ -> assert' "Error during decoding" false
{% endhighlight %}

As we can see in line 5 - *bind* *either* value from parsed file into
*push_event* name.
Next line - match both paths - happy path (Right - as "right is right") and
left.
If we have Left - then there's an error and we must fail - `assert false` is
enough.
For right value - let's write helper function that'll do checking for us.

Let's run it:
```
$ pulp test
* Build successful.
* Running tests...
* Tests OK.
```

Success! Code as always it it's
[tag](https://github.com/matma/github-linter/tree/step-3)
