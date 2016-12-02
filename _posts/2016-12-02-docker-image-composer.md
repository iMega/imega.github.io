---
layout: post
title: Update version composer 1.2.3
lang: en-US
description: This is docker image to run the composer
tags:
  - docker
  - image
  - composer
---
This is docker image to run the [composer](https://getcomposer.org).

<!--more-->

This is docker image to run the [composer](https://getcomposer.org).

[![](https://images.microbadger.com/badges/image/imega/composer.svg)](http://microbadger.com/images/imega/composer "Get your own image badge on microbadger.com") [![CircleCI](https://circleci.com/gh/imega-docker/composer.svg?style=svg)](https://circleci.com/gh/imega-docker/composer) [![GitHub stars](https://img.shields.io/github/stars/badges/shields.svg?style=social&label=Star&maxAge=2592000)](https://github.com/imega-docker/composer)

Image size: 37 MB

From image: alpine:3.4

PHP 7.0.13

Composer version: 1.2.3

## Usage

```
$ docker run --rm -v /path/to/project/:/data imega/composer install --no-dev --ignore-platform-reqs --no-interaction
```

## Alpine Packages
  - musl (1.1.14-r14)
  - zlib (1.2.8-r2)
  - libcrypto1.0 (1.0.2j-r0)
  - busybox (1.24.2-r12)
  - ca-certificates (20160104-r4)
  - libssl1.0 (1.0.2j-r0)
  - libssh2 (1.7.0-r0)
  - libcurl (7.51.0-r0)
  - expat (2.1.1-r1)
  - pcre (8.38-r1)
  - git (2.8.3-r0)
  - libressl2.4-libcrypto (2.4.4-r0)
  - libressl2.4-libssl (2.4.4-r0)
  - libressl(2.4.4-r0)
  - php7-common (7.0.13-r1)
  - libxml2 (2.9.4-r0)
  - php7 (7.0.13-r1)
  - php7-xml (7.0.13-r1)
  - php7-dom (7.0.13-r1)
  - php7-json (7.0.13-r1)
  - php7-mbstring (7.0.13-r1)
  - php7-openssl (7.0.13-r1)
  - php7-phar (7.0.13-r1)
  - php7-zlib (7.0.13-r1)

## PHP Modules
  - Core
  - date
  - dom
  - fileinfo
  - filter
  - hash
  - json
  - libxml
  - mbstring
  - openssl
  - pcre
  - Phar
  - Reflection
  - SimpleXML
  - SPL
  - standard
  - tokenizer
  - xml
  - xmlwriter
  - zlib
