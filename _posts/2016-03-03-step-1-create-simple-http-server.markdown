---
published: true
title: First day with PureScript
layout: post
---

After installation & run `purs init`, `bower install purescript-node-http` I
tried to undestand what's going on :smile:.

My idea was to start node.js http server using standard [http
module](https://nodejs.org/api/http.html) in the simplest form.

After few fails, I copied the example from purescript-node-http
[repo](https://github.com/purescript-node/purescript-node-http/blob/master/test/Main.purs).

After fixing some warnings it got run.

So I removed all but bare minimum which gave code as below:

```purescript
module Main where

import Prelude

import Node.HTTP
import Node.Stream
import Node.Encoding

import Data.Foldable (foldMap)

import Control.Monad.Eff.Console

main = do
  server <- createServer respond -- (1)
  listen server 8080 $ void do   -- (2)
    log "Server is listening!"
  where
  respond req res = do           -- (3)
    log "Incoming request"
```

And to test it we'll use `curl`:
 - on first terminal - `pulp run`
 - on second terminal - `curl :8080`

And the result is - displaying *"Incoming request"* :smiley_cat:.

Main finding of the day:
 - (1) - call
   [createServer](https://nodejs.org/api/http.html#http_http_createserver_requestlistener)
and provide `respond` callback (3). Assigns it to server which is const,
because I heard that all data is const in PureScript. Maybe I'm wrong.

`log` just logs :smile:.

Question of the day:
> What the f... are those *$* and *= do*

Bonus question:
> What is *void* - (2)

Code can be found at this [GitHub tag](https://github.com/matma/github-linter/tree/step-1).
