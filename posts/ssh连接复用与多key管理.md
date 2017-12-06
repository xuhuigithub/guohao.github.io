---
title: ssh连接复用与多key管理
date: 2017-12-06 01:50:09
tags: ssh
---

## key生成

### 公司key

```
$ ssh-keygen -t rsa
```
回车,随便起个文件名,这里以id_rsa_bidu为例

### github key
```
$ ssh-keygen -t rsa -b 4096 -C  "yourEmail@address"
```
回车,随便起个名,这里以id_rsa_github为例


## 拷贝公钥

公司的
```
$ cat ~/.ssh/id_rsa_bidu | pbcopy
```

github
```
$ cat ~/.ssh/id_rsa_github | pbcopy
```

## 配置config文件
```
$ vi ~/.ssh/config
```

添加以下内容
> Host * 
    ControlMaster auto 
    ControlPath ~/.ssh/%h-%p-%r 
    ControlPersist yes
Host baidu.com
    HostName *.baidu.com
    User guohao02
    IdentityFile ~/.ssh/id_rsa_bidu
    PreferredAuthentications true
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_github

其中,controlMaster配置用于复用连接,HostName用于配置key作用域名。

## 测试
```
$ ssh -Tv git@github.com
```

公司的方法类似,不再列出。
