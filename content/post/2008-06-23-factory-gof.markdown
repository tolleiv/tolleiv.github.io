---
author: tolleiv
categories:
- creational
- factory
- pattern
comments: true
date: 2008-06-23T18:20:00Z
slug: factory-gof
title: Factory [GoF]
wordpress_id: 582
---

Do you remember the [Fa](http://www.cookiepattern.com/2008/06/faade-gof.html)[ç](http://www.cookiepattern.com/2008/06/faade-gof.html)[ade pattern](http://www.cookiepattern.com/2008/06/faade-gof.html) post? Sure you do :) - I described a child which asked his granny whether she could bake some cookies. So how this happens in a formal way was describe within the[ Fa](http://www.cookiepattern.com/2008/06/faade-gof.html)[ç](http://www.cookiepattern.com/2008/06/faade-gof.html)[ade pattern](http://www.cookiepattern.com/2008/06/faade-gof.html) post ;) but the basic action "to ask for sth." is described here...  
  
As you might remember we had a child, probably a lazy one, which was in the mood to eat some cookies but since the cookie tin was empty it asked his granny to bake some new. In OOP thats nearly the same - when you'd to have a (complex) object and if you don't already have one you need someone who creates it or you do it yourself but since you need remember all the steps to create it, that's something for programmers who likes to type :P  
  
So normally you create a method which wraps up all the code to create the complex object. Or maybe you have  a Aggregate and you don't want that everyone needs to know how it is build up then this method should be the only way to access the Aggregate.  
  
You see now how you can benefit when you have a Factory available - but there are also some drawbacks. Factories tend to be hard to extend - this happens because the factory has a very deep (mostly hard-coded) knowledge of the objects it creates and normally all the wiring happens statically. If you're looking for a dynamic way to wire up your objects you might end making heavy use of reflection. In normal projects this might be a bit too much ;)  
  
A normal factory method, for a cookie which requires to have 3 flavors, a predefined size and that it's baked before anyone consumes it, could look like this:  


  
class Cookie {   
  
 // ...   
  
 /*  
  * Create a Cookie object which consists  
  * of 3 flavors and has a defined size.  
  */   
 public function getInstance() {  
     $c = new Cookie();  
     $c->setFlavor(Cookie::randomFlavor());  
     $c->setFlavor(Cookie::randomFlavor());  
     $c->setFlavor(Cookie::randomFlavor());  
     $c->setSize(Cookie::randomSize());  
     $c->bakeIt();  
     return $c;       
 }  
  
}  


  
If your software contains some complexity where you want to switch the used objects (created by a factory) without modifying the clients, which make use of the factory, you'll end up with the so called Abtract factory.  
  
Let's say you have two types of Cookies - the first one is a high-quality cookie with a bunch of different flavors and the second one is a normal chocolate-cookie. The high-quality cookie is created by your granny (slow but mind-blowing taste), the cookies with the lower quality are the result of mass-production. Your "client" is a wrapping machine which does everything to deliver the cookies to some customers.  
So the wrapping machine won't care what kind of cookies are prepared for delivering and so it doesn't matter who the creator is - the only thing it has to know is how it can access the cookies... some kind of interface.  
So the entire szenario could look like this:  


  
class Cookie {  
  
}  
  
interface CookieFactory {  
 abstract function createCookie();  
}  
  
class Granny implements CookieFactory {  
 public function createCookie() {  
     $cookie = new Cookie();  
     // do some wierd cookie-creation stuff //  
     return $cookie;  
 }  
}  
  
class MassProduction implements CookieFactory {  
 public function createCookie() {  
     return new Cookie();  
 }  
}  
  
class wrappingMachine {  
 public function run(CookieFactory $factory) {  
     $i = 6;  
     $package=array();  
     while($i--) {  
         $package[] = $factory->createCookie();           
     }  
     return $package;  
 }  
}  
  


  
Client-code could look like this;  
  


  
$wrapper = new wrappingMachine();  
$highQualityCookiePackage = $wrapper->run(new Granny());  
$lowQualityCookiePackage = $wrapper->run(new MassProduction());  
  


  
Thats mostly what you can achieve with some normal Factories in the GoF-meaning. Some other interesting ways of object creation are the [Plugin pattern](http://www.cookiepattern.com/2008/06/plugin-poeaa.html) and the  [Prototype pattern](http://www.cookiepattern.com/2008/05/prototype-gof.html). :)  
  

