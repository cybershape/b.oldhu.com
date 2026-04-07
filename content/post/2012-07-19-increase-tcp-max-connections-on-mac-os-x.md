---
date: 2012-07-19T00:00:00Z
draft: false
status: publish
tags:
- node
- nodejs
- Mac
- osx
- tcp
- network
title: Increase TCP max connections on Mac OS X
type: post
url: /2012/07/19/increase-tcp-max-connections-on-mac-os-x/
---

## Reason

I was trying to benchmark a TCP based file server on Mac OS X. The server is running latest Node.js (0.8.2), while the client to push the server is written in [go](http://golang.org). 'Go' can start thousands of concurrent goroutines easily. I got error when there are hundreds of concurrent clients.

## Steps

 1. increate max open files
```
$ sysctl -a | grep files
kern.maxfiles = 12288
kern.maxfilesperproc = 10240
```
  kern.maxfiles and kern.maxfilesperproc were small numbers, they need to be increased:
```
$ sudo sysctl -w kern.maxfiles=12288
$ sudo sysctl -w kern.maxfilesperproc=10240
```
  after this, you can increase your account's limit by ulimit -n:
```
$ ulimit -n 10240
```
 2. increate max sockets
```
$ sysctl -a | grep somax
kern.ipc.somaxconn: 2048
```
  It was a small number and need to be increased:
```
$ sudo sysctl -w kern.ipc.somaxconn=2048
```

## References
  * [Performance Tuning the Network Stack on Mac OS X « Rolande's Ramblings](http://rolande.wordpress.com/2010/12/30/performance-tuning-the-network-stack-on-mac-osx-10-6/)
  * [A caution on modifying kern.maxfiles values ](http://hints.macworld.com/article.php?story=20050901071836366)
