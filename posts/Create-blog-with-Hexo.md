---
title: Create blog with Hexo
date: 2017-09-22 23:43:47
tags: website
---
## What is Hexo?
Hexo is a fast, simple and powerful blog framework. You write posts in Markdown (or other languages) and Hexo generates static files with a beautiful theme in seconds.

## Installation
### Requirements
- [Node.js](http://nodejs.org/)
- [Git](http://git-scm.com/)
<!-- more --> 

If your computer already has these, congratulations! Just install Hexo with npm:
```
$ npm install -g hexo-cli
```

### Install Node.js
<p>The best way to install Node.js is with [Node Version Manager](https://github.com/creationix/nvm).
<p>Thankfully the creators of nvm provide a simple script that automatically installs nvm:
<p>cURL:

```
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
Wget

```
$ wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
<p>Once nvm is installed, restart the terminal and run the following command to install Node.js:

```
$ nvm install stable
```
<p>Alternatively, download and run [the installer](http://nodejs.org/).

### Install Hexo
Once all the requirements are installed, you can install Hexo with npm:

```
$ npm install -g hexo-cli
```

## Setup
Once Hexo is installed, run the following commands to initialise Hexo in the target `<folder>`.

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```


## Start
Start server

```
hexo server -p 4000
```
