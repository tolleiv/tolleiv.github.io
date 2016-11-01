---
categories: devops
comments: true
date: 2015-01-05T22:18:26Z
title: Post mortem documentations or how to build knowledge during failures
---

Few months ago I tried to get my head around Post-Mortem documentations and found it particularly hard to fill the gap between the publicly available documentations and my aim to have company internal documentations which teams could use to share knowledge and learn from past mistakes. During my research I came across lot’s of publicly available information which helped me to dive into the topic. But unfortunately information was widely distributed and I though that sharing my link collection could help to shorten your way a bit.

## Basics

Some good reads if you want to learn what Post-Mortems are:

* [Postmortem reviews: purpose and approaches in software engineering](http://www.uio.no/studier/emner/matnat/ifi/INF5180/v10/undervisningsmateriale/reading-materials/p08/post-mortems.pdf)  (Time invest 30 mins)
* O’Reilly "[Web Operations - Keeping the Data On Time](http://shop.oreilly.com/product/0636920000136.do)“ — Chapter 13 "How to Make Failure Beautiful: The Art and Science of Postmortems"
 (Time invest 20 mins)
* [The Project Post-Mortem: A Valuable Tool for Continuous Improvement](http://www.cdlib.org/cdlinfo/2010/11/17/the-project-post-mortem-a-valuable-tool-for-continuous-improvement/) (Time invest 5-15 mins)

## Foundations

If you want to look further into the topic, you’ve to deal with human error and failure. These will give you some idea how large this topic is:

* [How Complex Systems Fail](http://web.mit.edu/2.75/resources/random/How%20Complex%20Systems%20Fail.pdf) - Richard Cook (Time invest 15mins)
* Velocity 2012 (Video): [How Complex Systems Fail](http://www.youtube.com/watch?v=2S0k12uZR14) - Richard Cook (Time invest 30 mins)
* [Fallible Humans](http://www.indecorous.com/fallible_humans/) (Time invest 35 mins)
* [The Human Side of Postmortems](http://www.oreilly.com/webops-perf/free/the-human-side-of-postmortems.csp) (Time invest 45 mins)
* [Field Guide to Understanding Human Error](http://www.amazon.de/Field-Guide-Understanding-Human-Error/dp/0754648265) - Sidney Dekker

## Instructions

Adding up on top of that, there are lot’s of blog-posts, interviews and descriptions on how post-mortems should be conducted:

* [Say Goodbye to Post-Mortems. Say Hello to Effective Problem Management](http://www.cmg.org/wp-content/uploads/2009/06/8022.pdf) -(Time invest 30mins)
* (Video) [John Allspaw (Etsy) Interview - Velocity Santa Clara 2014](http://youtu.be/ciIT2r_j050) (Time invest 30 mins)
* (Slides) [How to Run a Post-Mortem With Humans (Not Robots)](http://velocityconf.com/velocity2013/public/schedule/detail/28251) (Time invest 10 mins)
* [Don’t Repeat your Mistakes: Conducting Post-mortems](http://blog.pusher.com/dont-repeat-your-mistakes-conducting-post-mortems/) (Time invest 7 mins)
* [Extending Agile Methods: Postmortem Reviews as Extended Feedback](http://dimsboiv.uqac.ca/Cours/C2013/8INF851_Aut/tp_paper/paper/postmortem.pdf) (Time invest 20 mins)
* (Slides) [It’s not your fault](http://www.slideshare.net/mobile/fullscreen/jhand2/its-not-your-fault-blameless-post-mortems/3) (Time invest 5 mins)
* (Video) [How to write an Incident Report / Postmortem](https://sysadmincasts.com/episodes/20-how-to-write-an-incident-report-postmortem) (Time invest 5 mins)
* [The Three Ingredients of a Great Postmortem](http://blog.travis-ci.com/2014-06-26-three-ingredients-to-a-great-postmortem/) (Time invest 5 mins)
* [Blameless PostMortems and a Just Culture](https://codeascraft.com/2012/05/22/blameless-postmortems/) (Time invest 10 mins)
* [Morgue: Helping Better Understand Events by Building a Post Mortem Tool - Bethany Macri](http://vimeo.com/77206751) (Time invest 33 mins)
* (Slides) [Human Factors and PostMortems](http://www.unwiredcouch.com/talks/human-factors-postmortems/) (5 mins)
* [Blameless Post-Mortems](http://www.infoq.com/news/2014/07/blameless-post-mortems) (Time invest 5 mins)
* [What Adopting Blameless Post-Mortems Has Taught Me About Culture](http://www.paperplanes.de/2014/6/20/what-blameless-postmortem-taught-me.html) - Mathias Meyer (Time invest 7 mins)
* [DevOps: To increase reliability you need to have more outages](http://everythingsysadmin.com/2012/09/more-outages.html) (Time invest 7 mins)
* [What blameless really means](http://www.jessicaharllee.com/notes/what-blameless-really-means/) (Time invest 3 mins)
* [Postmortems, sans finger-pointing: The O’Reilly Radar Podcast](http://radar.oreilly.com/tag/blameless-postmortem) (Time invest 30 mins)

## Tools

Once you discovered all of that and you want to apply it in your team, there are even some tools available:

* Etsy Morgue ([Github](https://github.com/etsy/morgue))
* [Post Mortem Documents](http://www.teresadietrich.net/?page_id=37) (incl. Excel template)
* [Post Mortem Template](http://de.slideshare.net/fattofatt/post-mortem-report)

With all of those you get a great insight in what type of culture you should establish in your team and essentially this makes up a good internal documentation and brings up good input for public statements. Which kind of filled the gap for me.

## Archives
Finally there’s a long list of links to existing documentations. First and foremost there’s a never ending list of post-mortem documentations on Tumblr:  [fuckyeahpostmortems.tumblr.com](http://fuckyeahpostmortems.tumblr.com)

Looking closer into these, there are some examples which span across some well known companies:

* [MailGun](http://blog.mailgun.com/what-happened-yesterday-and-what-we-are-doing-about-it/)
* [PagerDuty](https://blog.pagerduty.com/2012/06/outage-post-mortem-june-14/)
* [Github](https://github.com/blog/1759-dns-outage-post-mortem)
* [Heroku Application Outage](https://status.heroku.com/incidents/15)
* Google App Engine Outage ([1](https://groups.google.com/forum/m/#!topic/google-appengine/p2QKJ0OSLc8)),([2](http://googledevelopers.blogspot.ca/2013/05/google-api-infrastructure-outage_3.html))
* [GetChef](https://www.getchef.com/blog/2014/07/10/berkshelf-v2-outage-postmortem/)
* [ZDNet](http://www.zdnet.com/blog/btl/post-mortem-our-site-fail-wednesday-and-what-went-wrong/3023)
* [Joyent](https://www.joyent.com/blog/postmortem-for-outage-of-us-east-1-may-27-2014)
* [AWS](http://aws.amazon.com/de/message/65648/)
* [StackExchange](http://stackstatus.net/post/97322396704/outage-postmortem-sepember-11th-2014)
* [DNSimple](http://blog.dnsimple.com/2014/12/incident-report-ddos/)

Other industries of course also have postmortem documentations or lessons learned which they share:

* [Fire fighting](http://www.wildfirelessons.net)
* [Indy game developers](http://www.pixelprospector.com/the-big-list-of-postmortems/ )
* [VC / Company startup failures](http://www.chubbybrain.com/blog/startup-failure-post-mortem/)

***

#### Offtopic
I’ve also learned that there are programming techniques which enable you to debug software fails on the algorithmic levels. E.g. for [NodeJS](http://dtrace.org/blogs/dap/2012/01/13/playing-with-nodev8-postmortem-debugging/) or [Python](http://bashdb.sourceforge.net/pydb/pydb/lib/node36.html).

