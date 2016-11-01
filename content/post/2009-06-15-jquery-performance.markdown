---
author: tolleiv
categories:
- Coding
comments: true
date: 2009-06-15T06:53:58Z
slug: jquery-performance
tags:
- jQuery
title: jQuery performance
wordpress_id: 117
---

jQuery 1.3 made a huge step especially towards better performance. But it's not only the framework, from time to time it's the developer who should care about performance too. Artzstudio.com made up a really good summary of [how you should change your coding-style to achieve a higher performance with jQuery](http://www.artzstudio.com/2009/04/jquery-performance-rules/).

In my opinion the most important things are:



	
  1. [Use ID selectors whenever possible](http://www.artzstudio.com/2009/04/jquery-performance-rules/#descend-from-id) since this can be directly mapped to **getElementById()** it much faster then all the other selectors

	
  2. [Use chaining whenever possible](http://www.artzstudio.com/2009/04/jquery-performance-rules/#harness-chaining) since it will reuse the already selected element whichout additional selectors

	
  3. [Limit direct DOM manipulation](http://www.artzstudio.com/2009/04/jquery-performance-rules/#limit-dom-manipulation) obviously browsers don't like it therefor you should avoid it ;)

	
  4. Distinguish between [**$(window).load()** and **$(documen).ready()**](http://www.artzstudio.com/2009/04/jquery-performance-rules/#defer-to-window-load)


Besides this single resource there's also the [list with 25 jQuery tips from tvidesign.co.uk ](http://www.tvidesign.co.uk/blog/improve-your-jquery-25-excellent-tips.aspx)which contains lot's of useful stuff. And of course there are ton's of cheat sheets lying around in the web - you should at least use one ;) .

[(http://acodingfool.typepad.com/blog/jquery-13-cheat-sheet.html](http://acodingfool.typepad.com/blog/jquery-13-cheat-sheet.html))
([http://www.gscottolson.com/weblog/2008/01/11/jquery-cheat-sheet/](http://www.gscottolson.com/weblog/2008/01/11/jquery-cheat-sheet/))
([http://colorcharge.com/jquery/](http://colorcharge.com/jquery/))
([http://www.gmtaz.com/index.php/jquery-13-cheatsheet-wallpaper/](http://www.gmtaz.com/index.php/jquery-13-cheatsheet-wallpaper/))
