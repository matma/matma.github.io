---
published: true
title: Github linter bot
layout: post
---
## Motivation

Writing code can be a difficult task. So many things that we need to look at. Hopefully there are some tools that can help us to spot obvious bugs. They're called *linters*. Almost [any language](https://en.wikipedia.org/wiki/List_of_tools_for_static_code_analysis) has it's dedicated linter.

On the other hand - every team has people that are interested in keeping code clean and people that do not have skills or interest to do so. But fortunately GitHub and similar tools allows us to force that people to keep code clean!

## Project description

The goal is to write a *[Github linter bot](https://github.com/matma/github-linter)* Following project will show basics of [Github webhooks](https://developer.github.com/webhooks/), how can we use it together with [ESLint](http://eslint.org/) to create linter for JavaScript. But our bot should be written in form that'll allow us to plug any linter to it.

I won't explain basic stuff, how to install or configure things. Google is your friend!

## Basic workflow

At every *push*, Github can send HTTP request on given address with specific [payload](https://developer.github.com/v3/activity/events/types/#pushevent). This is the initial point of our workflow. We need to listen for incoming events, get information about changes from it, get changed code and lint it. That's it :) Sounds really easy. And that's it. 

## Technology stack

I want to complicate things a bit and write it in [PureScript](http://www.purescript.org/). It's very [Haskell](https://www.haskell.org/) like language that's being compiled to JavaScript.

Why? Because I believe that [Functional Programming](https://en.wikipedia.org/wiki/Functional_programming) is the future of professional programming and if we get on this train later then we can still find free places in 1st class car :) 

Also it's just f**** awesome to cut a lot of possible bugs just by using *the right* tool :D

At the very bottom all code will be executed on [node.js](https://nodejs.org/en/). I'll use some additional libraries for PureScript and node, but I want this code to be as clean as possible. 

I have no professional experience in Haskell nor PureScript. I can make rookie mistake then. But even [best guys makes mistakes](https://www.youtube.com/watch?v=DisD9ftUyCk) :)

At the end - repository address: [https://github.com/matma/github-linter](https://github.com/matma/github-linter).