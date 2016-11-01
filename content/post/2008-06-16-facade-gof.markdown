---
author: tolleiv
categories:
- facade
- pattern
- php
comments: true
date: 2008-06-16T23:33:00Z
slug: facade-gof
title: Façade [GoF]
wordpress_id: 578
---

If a child is in the mood to eat a fresh cookie it normally asks his granny for one. Like this:  
  
Child: Grannyyyyy?  
Granny: Yes my dear?  
Child: Could you mix about 2 cups of sugar, 1/2 cup of butter and 1/2 cup of milk in a saucepan and boil it for a minute. And could you, after you removed the saucepan from the heat, mix in some cocoa powder and 3 cups quick cooking oats and form some cookies? Pleeease?  
  
Hm you're right that's not very realistic - it's more like: "Grannyyyy? Could I have a cooookieee pleeeease?" ....the granny knows what to do and the child will get it's cookie  
  
So what happens if you have a pice of software which provides <strike>cookies</strike> some services for other parts of your software? The most people (especially programmers) are lazy and they won't remember all the details of a complex structure. They remember the place or object which can run a certain task but there's no need to know the deeper structure of that object - the only thing which is important if your using a service of an object is that it doesn't fail.  
So the Façade pattern is a structural pattern which more or less describes that you create a object with an simplified interface, so that you can hide complex structures. You can also use a Façade to wrap up some poorly designed APIs into a single well designed API. And the larges benefit of a Façade object is that it makes APIs more readable and therefore enables flexible and easy development.  
  
So maybe you're missing the example-code for this pattern, but since it's not that concrete as others, I've not implemented a  special example for it. But I already used a Facade in some way, if you look at the [Specification pattern](http://www.cookiepattern.com/2008/05/specification-ddd.html), you'll find the functions getSmallChocolateCookies() and getLargeCookies(). Both show in a tasty way what I described  here :)
