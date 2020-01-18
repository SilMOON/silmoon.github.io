---
layout: post
title: Check the usage of ports in Linux
categories: [Linux]
tags: [Linux, network]
fullview: true
---

We can use the `netstat -anp` command to check active connections and port usage.

`-a` shows all active connections.

`-n` shows applications an port number in numbers.

`-p` lists processes(pid) which occupied the port. And we can simply `kill` them using the pid if we need.