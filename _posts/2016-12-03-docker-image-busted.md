---
layout: post
title: Docker image to run the Busted
lang: en-US
description: This is docker image to run the Busted
tags:
  - docker
  - image
  - Busted
  - testing
  - framework
---
This is docker image to run the [busted](http://olivinelabs.com/busted/). Busted is a unit testing framework with a focus on being easy to use.

<!--more-->

This is docker image to run the [busted](http://olivinelabs.com/busted/).

[![](https://images.microbadger.com/badges/image/imega/busted.svg)](http://microbadger.com/images/imega/busted "Get your own image badge on microbadger.com") [![CircleCI](https://circleci.com/gh/imega-docker/busted.svg?style=svg)](https://circleci.com/gh/imega-docker/busted)

Image size: 2.6 MB

From image: alpine:3.4

Lua 5.1

Busted version: 2.0.rc12-1

## Usage

```
$ docker run --rm -t -v `pwd`:/data imega/busted
```
