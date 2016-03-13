---
published: false
title: HTTP server for incoming payload
layout: post
---

# Simple server

So the main idea is to write an [eslint](http://eslint.org) bot for GitHub.
It'll be listening on given port any POST request and deal with it. GitHub has
[webhooks guide](https://developer.github.com/webhooks/), which API provided.

Main workflow looks like this (it's based on excellent work by
[Bernardstanislas](https://github.com/Bernardstanislas/eslint-bot))

 - run node server on given port
 - listen incoming events
 - get commits given in this event
 - run linter on it
 - push comments to commit using GitHub API

Today I'll try to 'mimic' events using CURL.

We have our simple server that's listening on port 8080. I want to listen only
for POST request. So only post on '/' should be handled. Otherwise - return
405.

I don't know how to unit test this yet :) So I'll use CURL.

Let's start with 405 always. It should be easier. Probably
[setStatusCode](https://github.com/purescript-node/purescript-node-http/blob/master/docs/Node/HTTP.md#setstatuscode)
function should be used here. Let's try.

{% highlight haskell linenos %}
main = do
  server <- createServer respond
  listen server 8080 $ log "Server started"
  where
  respond req res = do
    setStatusCode res 405
    end (responseAsStream res) (return unit)
{% endhighlight %}

So far so good. `psci run` and on second terminal:

```
$ curl -v :8080
* Rebuilt URL to: :8080/
*   Trying ::1...
* Connected to  (::1) port 8080 (#0)
> GET / HTTP/1.1
> Host: :8080
> User-Agent: curl/7.43.0
> Accept: */*
>
< HTTP/1.1 405 Method Not Allowed
< Date: Thu, 21 Jan 2016 20:08:09 GMT
< Connection: keep-alive
< Content-Length: 0
<
* Connection #0 to host  left intact
```

As we can see - returns 405!

Using function from module we set status code on stream - line 6. Easy.
Following node.js practice - we end a stream (first we need to get a writable
stream).

`(return unit)` has something to do with effects.
There's a [blog post](http://www.purescript.org/learn/eff/)
I should get familiar with. It'll take probably whole day. Some future day :)

Just one thing at the end - `(return unit)` *probably* means - we do not want
to use any effects.

# Proper server

We want to handle "POST" method, so we need to modify the code a bit, but
it's rather straightforward - we can just use
[case](https://leanpub.com/purescript/read#leanpub-auto-case-expressions)

{% highlight haskell linenos %}
main = do
  server <- createServer respond
  listen server 8080 (return unit)
  where
  respond req res = do
    let method = requestMethod req
    case method of
      "POST" -> do
         setStatusCode res 200
         void $ pipe (requestAsStream req) (responseAsStream res)
      _     -> do
         setStatusCode res 405
         end (responseAsStream res) (return unit)
{% endhighlight %}

Easy as pie. If it's "POST" - pipe input to output. If not - return 405.

Do some testing:
```
$ curl localhost:8080 --data '{ "test_key": "test_value"}' | jq
{
  "test_key": "test_value"
}
```

Input copied to output, as we expected!

# Summary

First part of our puzzle is in place. Server is up & running and ready to
receive payload from GitHub.

Code is at the [step2](https://github.com/matma/github-linter/tree/step-2) tag.

