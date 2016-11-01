---
author: tolleiv
comments: true
date: 2009-04-28T16:22:55Z
slug: shell-command-translare
tags:
- bash
- shell
title: 'shell command: translate '
wordpress_id: 33
---

> "Translate, squeeze, and/or delete characters from standard input,
writing to standard output."


... that's the manpage description of the shell-command I just found (with the help of a good friend) ... and besides sed this is a very handy way to do simple string-operations on stdin.

He used it to replace (old) Mac newline characters with Unix newline charaters within files (as seen on [wikipedia](http://en.wikipedia.org/wiki/Newline)):
`tr '\r' '\n' < old.file > new.file`

Btw I still think that it's stupid that users still have all the hazzle with [newline charaters](http://en.wikipedia.org/wiki/Newline) - why can't the OS-developers just recognize all types of newlines? :(
