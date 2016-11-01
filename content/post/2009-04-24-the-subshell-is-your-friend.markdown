---
author: tolleiv
comments: true
date: 2009-04-24T21:10:29Z
slug: the-subshell-is-your-friend
tags:
- shell
title: the subshell is your friend
wordpress_id: 43
---

Today I tried to find a way to track the time of a shell-command and to log that runtime into a file. I was already aware of the **time **command which is a bash build-in. Due to that it passes its output directly to the user without using either stdout or stderr and therefore there's no "easy" way to redirect the output directly into a file.

But bash als provides subshells and in this case that's how you can use the output :P (which is passed on stderr in this case).

`(time sleep 5) 2> time.log`
