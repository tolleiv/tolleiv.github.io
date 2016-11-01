---
author: tolleiv
categories:
- behavioral
- "null"
- pattern
- php
comments: true
date: 2008-07-01T17:58:00Z
slug: null-object-pattern
tags:
- behavioral
- "null"
- pattern
- php
title: Null-Object Pattern
wordpress_id: 584
---

Very often you create a object with a factory and before you really start using it, you check if your factory really created a object or returned NULL. Or maybe you have a method where a object is passed in and in this situation you'll have to do this check also.

Instead of typing the "if(object == null)" phrase again and again, you could use the Null-Object pattern, you'll see that this can make some situations much clearer.

Basically Null-Object ensures that the client always receives a valid object for it's interaction, so that there's no need to do the check shown above again and again. This happens since your concrete Null-Object just shares the interface, or inherits from the same class as it's effective counterpart, but it's implementation just leaves out any effectiveness.
[
![](http://bp3.blogger.com/_l5fIZzJyYfc/SGpiDZ9m-rI/AAAAAAAAACo/nVuiUHE4E90/s400/nullobject_pattern.png)](http://bp3.blogger.com/_l5fIZzJyYfc/SGpiDZ9m-rI/AAAAAAAAACo/nVuiUHE4E90/s1600-h/nullobject_pattern.png) So a code-example could look like this:


class CookieFactory {
public function makeInstance() {
if(date('l')=='Monday') return new NullCookie();
return new RealCookie();
}
}

interface iCookie {
function getCalories();
}

class RealCookie implements iCookie {
protected $calories=250;
public function getCalories() {
return $this->calories;
}
}

class NullCookie implements iCookie {
public function getCalories() {
return 0;
}
}



I think you can imagine what happens when you make use of the CookieFactory - diet on monday ;) 

There're also some disadvantages, your clients normally don't have a chance to react that there's something special happening, also the clients must "share" the same expectation what "do nothing" means, the number of required Null-Objects might be very large and unhandy and the Null-Object shares a very deep knowledge with the real one, so that it might be a large effort to create it if the real object is complex too.

I'm so glad that today is tuesday :P
