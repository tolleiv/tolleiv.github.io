---
author: tolleiv
categories:
- errors
- php
comments: true
date: 2008-06-11T20:28:00Z
slug: inherit-abstract-functions
title: Inherit abstract functions....
wordpress_id: 577
---

A error I find from time to time when I work with inheritance is this one:  


  
Fatal error: Can't inherit abstract function Cookie::setFlavor() (previously declared abstract in ChocolateCookie) in ...  


  
This happens when you try to define multiple abstract classes or interfaces with the same abstract functions - it's right that this errors shows up, but today I stuck on this error for a while because I did not see the reason for that - so that's a reminder for me ;)  
  
And the tasty example to reproduce this error:  


  
  
abstract class Cookie {  
abstract public function setFlavor($flavor);  
}  
  
abstract class ChocolateCookie extends Cookie {  
abstract public function setFlavor($flavor);  
}  
  


  
There are also some rejected bugreports about that :P [#35057](http://bugs.php.net/bug.php?id=35057) [#35832](http://bugs.php.net/bug.php?id=35832) [#41145](http://bugs.php.net/bug.php?id=41145)  

