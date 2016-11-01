---
author: tolleiv
categories:
- creational
- pattern
- prototype
comments: true
date: 2008-05-27T23:17:00Z
slug: prototype-gof
tags:
- creational
- pattern
- prototype
title: Prototype [GoF]
wordpress_id: 568
---

Imagine a cookie-oven which produces tasty cookies with chocolate crumbles. How do you ensure that the 1000th cookie still has the same taste as the first?
You might think that this is an easy task - just write down the recipe and follow the described steps...you know the result in real life - the 1000th cookie normally tasts like the 1st but you always had the "overhead" to read the recipe and go through the steps again and again.
In OOP it's much easier to follow the recipe just instantiate a new Object and  there you go... no matter if it's the 1st or the 1000th - it'll always taste look similar.
But the "recipe-overhead" is still there in a way and especially when you have larger objects whose construction is time-consuming you might want to somehow get rid of it. And that's where a Prototype can help you out - you just create the first Cookie Object and then you use the handy magic method __clone to create new objects.
Instead of just using __clone the pattern suggests a class (some kind of a factory-class) so that you can also encapsulate the creation of the objects (and also possible adjustments you might want to make after the creation/clone).

So the example just shows a cookie-machine which makes use of the prototype-pattern to create new cookies (depending on the cookie you throw in before)... yummy


[![](http://bp0.blogger.com/_l5fIZzJyYfc/SDrXY97tyhI/AAAAAAAAABg/1sAhpbVe2kI/s400/prototype_pattern.png)](http://bp0.blogger.com/_l5fIZzJyYfc/SDrXY97tyhI/AAAAAAAAABg/1sAhpbVe2kI/s1600-h/prototype_pattern.png)






abstract class Cookie {
function __clone() {    }
abstract public function printFlavor();
}

class CoconutCookie extends Cookie {
public function printFlavor() {
echo 'Coconut Flavor<br/>';
}
}
class ChocolateCookie extends Cookie {
public function printFlavor() {
echo 'Chocolate Flavor<br/>';
}
}

class CookieMachine {
protected $cookie;
public function __construct(Cookie $cookie) {
$this->cookie = $cookie;
}
public function makeCookie() {
return clone $this->cookie;
}
}


The client-code can look like this:



$coconutCookie = new CoconutCookie();
$coconutCookieMachine = new CookieMachine($coconutCookie);

$chocolateCookie = new ChocolateCookie();
$chocolateCookieMachine = new CookieMachine($chocolateCookie);

//while(true) {
for($i=0;$i<5;$i++) {
$coconutCookieMachine->makeCookie()->printFlavor();
$chocolateCookieMachine->makeCookie()->printFlavor();
}


