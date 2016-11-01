---
author: tolleiv
categories:
- Coding
comments: true
date: 2009-06-11T07:27:34Z
slug: my-jquery-132-update-experience
tags:
- javascript
- jQuery
title: my jQuery 1.3.2 update experience
wordpress_id: 113
---

Over the last days I did some small steps to clean up the code of my [TYPO3 imagemap extension](http://forge.typo3.org/projects/show/extension-imagemap_wizard). During that process I wanted to update jQuery from 1.2.6 to 1.3.2. Initially this went fine but after some testing I found this strange error:

`[Exception... "'Syntax error, unrecognized expression: #' when calling method: [nsIDOMEventListener::handleEvent]"  nsresult: "0x8057001e (NS_ERROR_XPC_JS_THREW_STRING)"  location: "<unknown>"`

Due to the *verbosity* of the message (location "<unknown>" ) it wasn't obvious for me that what's going wrong. But right after I found the [jQuery-bugtracker log #4323](http://dev.jquery.com/ticket/4323) _($('#' + myID) throws an exception in version 1.3.1 and 1.3.2 if myID is a zero length string)_ everything was clear. For some reason they've made the decission to add some strictness to their API so for the future always validate your selectors before you ask jQuery ;)
