---
author: tolleiv
categories:
- pattern
- prototype
- structural
comments: true
date: 2008-05-12T22:59:00Z
slug: decorator-gof
tags:
- pattern
- prototype
- structural
title: Decorator [GoF]
wordpress_id: 558
---

As the first pattern I'd like to introduce the decorator pattern - it's one of the [GoF]- structural patterns. It enables to extend the functionality of a existing method by wrapping a so called decorator-object.

So maybe you already know the situation ;) , your granny  is going to bake cookies and you think of how they gonna taste - so cookie is our main-object and the different additional spices and other options which refine the taste of the cookies are the decoration for it. The cookies, pardon main-objects, are fine without the decoration but they're much better with and the best thing is that you're able to combine the decorations... yummy

So that's how this would look like more technically:
[![](http://bp2.blogger.com/_l5fIZzJyYfc/SCjU0tPibZI/AAAAAAAAAAU/uuR_geevSdw/s400/decorator-pattern.png)](http://bp2.blogger.com/_l5fIZzJyYfc/SCjU0tPibZI/AAAAAAAAAAU/uuR_geevSdw/s1600-h/decorator-pattern.png)


    
    
    stract class Cookie {
        protected $flavor;
        public function __construct($flavor) {
            $this->flavor=$flavor;
        }    
        abstract public function descripeFlavor();
    }
    
    class GrannysCookie extends Cookie {
        public function descripeFlavor() {
            echo ‘<br></br>Granny baked a cookie which has a taste of ’;
            echo $this->flavor;
        }
    }
    
    abstract class CookieDecorator extends Cookie {
        protected $cookie;
        public function __construct(Cookie $cookie) {
            $this->cookie = $cookie;
        }    
        //abstract public function descripeFlavor();    
    }
    
    class FreshCookieDecorator extends CookieDecorator {
        public function descripeFlavor() {
            $this->cookie->descripeFlavor();
            echo ‘ which smells fresh from the oven’;
        }
    }
    
    class CrumbleCookieDecorator extends CookieDecorator {
        public function descripeFlavor() {        
            $this->cookie->descripeFlavor();
            echo ‘ it has tasty crumbles ’;
        }
    }
    
    $cookie = new GrannysCookie(‘chocolate’);
    $cookie->descripeFlavor();
    
    $crumbleCookie = new CrumbleCookieDecorator($cookie);
    $crumbleCookie->descripeFlavor();
    
    $freshCookie = new FreshCookieDecorator($cookie);
    $freshCookie->descripeFlavor();
    
    $freshAndCrumbledCookie = new FreshCookieDecorator($crumbleCookie);
    $freshAndCrumbledCookie->descripeFlavor();
    



[additional Information](http://en.wikipedia.org/wiki/Decorator_pattern)
