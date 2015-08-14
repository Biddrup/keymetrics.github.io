---
layout: docs
title: Contributing
description: Contributing to PM2
permalink: /docs/usage/contributing/
---

## Contributing/Development mode

To play with PM2, it's very simple:

```bash
$ pm2 kill   # kill the current pm2
$ git clone my_pm2_fork.git
$ cd pm2/
$ DEBUG=* ./bin/pm2 --no-daemon
```

Each time you edit the code, be sure to kill and restart PM2 to make changes taking effect.


**DEBUG="*"** Allows to display all possible debug logs in ~/.pm2/pm2.log

## Install PM2 development

```bash
$ npm install git://github.com/Unitech/pm2#development -g
```

## Launch the tests

[![Build Status](https://img.shields.io/travis/Unitech/PM2/master.svg?style=flat-square)](https://travis-ci.org/Unitech/PM2)
[![Build Status](https://img.shields.io/travis/Unitech/PM2/development.svg?style=flat-square)](https://travis-ci.org/Unitech/PM2)

Just try the tests before using PM2 on your production server

```bash
$ git clone https://github.com/Unitech/pm2.git
$ cd pm2
$ npm install  # Or do NODE_ENV=development npm install if some packages are missing
$ npm test
```

If a test is broken please report us issues [here](https://github.com/Unitech/pm2/issues?state=open)
Also make sure you have all dependencies needed. For Ubuntu:

```bash
$ sudo apt-get install build-essential
# nvm is a Node.js version manager - https://github.com/creationix/nvm
$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
$ nvm install v0.11.14
$ nvm use v0.11.14
$ nvm alias default v0.11.14
```
