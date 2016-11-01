---
author: tolleiv
categories:
- behavioral
- memento
- pattern
comments: true
date: 2008-05-13T07:59:00Z
slug: memento-gof
tags:
- behavioral
- memento
- pattern
title: Memento [GoF]
wordpress_id: 559
---

Let's say you got a cookie from a repository your granny and you're not sure if you like the new taste. Wouldn't it be cool if you could just try it and revoke the operation first bite if you don't like it?

In the world of OOP thats a easy job which can be handled by the so called memento pattern - you just save the inner state of the object  within a memento object and revoke you actions whenever you like...

As UML the example looks like this:
[![](http://bp2.blogger.com/_l5fIZzJyYfc/SClV6dPibdI/AAAAAAAAABA/bFyT-BEsqrU/s400/memento-pattern.png)](http://bp2.blogger.com/_l5fIZzJyYfc/SClV6dPibdI/AAAAAAAAABA/bFyT-BEsqrU/s1600-h/memento-pattern.png)
Show PHP Source Code

    
    
    class CookieMemento {
        protected $flavor,$size;    
        public function __construct($flavor,$size=100) {
            $this->flavor=$flavor;
            $this->size=$size;
        }
        public function getMemento() {
            return new CookieMemento($this->flavor,$this->size);
        }
        public function setMemento(CookieMemento $memento) {
            $this->flavor=$memento->flavor;
            $this->size=$memento->size;
        }    
    }
    
    class Cookie extends CookieMemento {
        
        public function eat($reduceValue) {     
            $this->size-=($reduceValue>$this->size)?$this->size:$reduceValue;        
        }    
    
        public function printStatus() {
            echo ‘This cookie is a ’.$this->flavor.‘ cookie which has a size of ’.$this->size.‘.<br></br>’;
        }
    }
    
    class Client { 
        public function run() {
            $theCookie = new Cookie(‘chocolate’,100);
            $theMemento = $theCookie->getMemento();
            echo $theCookie->printStatus();        
            $theCookie->eat(50);
            echo $theCookie->printStatus();       
            $theCookie->eat(50);
            echo $theCookie->printStatus();       
            echo ‘Hm I\’d like to eat it again …. *grin*<br></br>’;
            $theCookie->setMemento($theMemento);
            echo $theCookie->printStatus();
            echo ‘ *biggrin* ’;
        }
    }
    
    $cookieMonster = new Client();
    $cookieMonster->run();
    
