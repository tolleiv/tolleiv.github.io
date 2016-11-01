---
author: tolleiv
categories:
- Coding
comments: true
date: 2009-06-18T21:54:34Z
slug: improve-bash-cd-builtin
tags:
- bash
- shell
title: 'bash: small improvement for the "cd" builtin'
wordpress_id: 126
---

If you use the shell and walk around in directories wouldn't it be cool to have "cd ..." to move 2 levels up, "cd ...." to move 3 levels up ...? I'm not sure if there's an easier way to resolve it but the following lines work pretty nice so far and they just made it into my default .bashrc :P

    
    cd() {
    	if [[ "$1" =~ ^\.\.\.+$  ]]; then
    		cd `echo "$1" | sed 's/\./..\//g' | sed 's/^\.\.\///g'`
    	else
    		builtin cd "$1"
    	fi
    }



For different older shell/bash versions you might need quotes around the regex.

Btw [O'Reilly's "Learning the Bash"](http://books.google.de/books?id=WQCSxv9vfPkC&lpg=PA84&ots=OiT8hUe10J&dq=bash%20function%20with%20the%20same%20name%20as%20command&pg=PP1) is available within Google Books

_Edit: another very important bookmark for bashscripting is the [Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/)_
