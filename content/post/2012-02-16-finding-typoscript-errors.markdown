---
author: tolleiv
comments: true
date: 2012-02-16T16:02:31Z
slug: finding-typoscript-errors
tags:
- debug
- typo3
title: Finding TypoScript errors.
wordpress_id: 949
---

When you work on TypoScript templates in TYPO3, errors might show up in the TypoScript Object Browser. Within the error messages you'll see a more or less detailed error description with the related line number. Within most setups these line numbers won't relate to any of your sys_template records or TypoScript files directly. But they still provide value if you know how they help to find the right spot. As it's not too obvious how to find the right spot I've created a little screenshot series to guide you to the broken spots in your templates.

So that's what you might see in your TypoScript Object Browser:


[![The error message from your TypoScript Object Browser might look like this.](/uploads/2012/02/typoscript-error.png)](/uploads/2012/02/typoscript-error.png)


 Switching from there to the Template Analyzer:


[![Switching to the TypoScript analyzer](/uploads/2012/02/typoscript-analyzer.png)](/uploads/2012/02/typoscript-analyzer.png)




At the bottom of the Template Analyzer, you'll find a "Complete TS" section and a link ~which looks like normal text.




[![And you'll find a link to the fully concatenated TypoScript of your current page.](/uploads/2012/02/typoscript-analyzer-complete.png)](/uploads/2012/02/typoscript-analyzer-complete.png)




Clicking on that link will give you the entire concatenated TypoScript and here you'll also find that the line numbers finally match to the error message.




[![And you'll find that the line numbers are now what you saw in the error message before (might need some scrolling).](/uploads/2012/02/typoscript-analyzer-error.png)](/uploads/2012/02/typoscript-analyzer-error.png)




A well hidden gem which works most likely in all TYPO3 4.x versions ;)




* * *


Edit: In the meantime, Ingo's patch made it through the review process. So users of TYPO3 4.7 and above will find a nice and handy "Show details" link next to the error message. Makes it much much faster to find the broken spot. Thanks Ingo :) / ([@irnnr](http://twitter.com/irnnr))
