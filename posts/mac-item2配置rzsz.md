---
title: mac-item2配置rzsz
date: 2017-12-07 01:20:10
tags: mac
---

在日常开发中，我们经常需要上传文件到服务器或者从服务器下载文件。在Windows下SecureCRT为我们提供了很方便的上传下载工具sz与rz，但是mac下一般都是通过scp命令来完成的，虽然也很方便，但是有些场景下是不能使用的，比如目前公司登录服务器需要经过跳板机。本篇我们就介绍一下如何在mac下使用rz、sz上传下载文件。

### 安装lrzsz
```
$ brew install lrzsz
```

### 获取term2-zmodem脚本
```
$ cd /usr/local/bin
$ sudo wget https://raw.github.com/mmastrac/iterm2-zmodem/master/iterm2-send-zmodem.sh
$ sudo wget https://raw.github.com/mmastrac/iterm2-zmodem/master/iterm2-recv-zmodem.sh
$ sudo chmod 777 /usr/local/bin/iterm2-*
```

### 配置Triggers
打开Item2，点击`preferences` → `profiles`，选择某个`profile`，如Default，之后继续选择`advanced` → `triggers`，添加编辑添加如下triggers：

Regular Expression  |   Action   |   Parameters
---- | --- | --- |
rz waiting to receive.\*\*B0100   |  Run Silent Coprocess    | /usr/local/bin/iterm2-send-zmodem.sh
\*\*B00000000000000    | Run Silent Coprocess    | /usr/local/bin/iterm2-recv-zmodem.sh

# 参考
https://github.com/mmastrac/iterm2-zmodem

