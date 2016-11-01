---
author: tolleiv
comments: true
date: 2011-02-24T21:17:14Z
slug: release-reviews-templavoila-imagemap_wizard-workspaces
tags:
- templavoila
- typo3
title: Release reviews (templavoila, imagemap_wizard, workspaces)
wordpress_id: 747
---

I just push the TER upload button two times and in addition to that TYPO3 4.5.2 will be released tomorrow containing some nice workspaces updates. So here's a short summary what happened in the extension releases.

**TemplaVoila 1.5.4**
The current release focussed on 4.5 compatibility. It uses the new sys_language flag "format" to support sprites, it hooks into the new backend-form _(TCA)_ "layout" and add's it's fields to the right tabs within the backend forms and adjusts everything to work fine with the new CSRF mechanism.

Besides that bug fixes for the section index, performance improvements and a couple more are included.

_One thing in conjunction with 4.5 you should be aware of is that copied elements are hidden by default. In older versions hidden elements won't show up in the page module by default and therefore it might seem that nothing was copied, but that's not right. With 1.5.4 the default setting was changed so hidden elements will show up in the page module. Unfortunately the old setting (to skip hidden elements) might still be present in your session settings - so please either clear your session settings or use the "Advanced function" tab in the page module to change to setting and avoid confusion._

**Imagemap_wizard 0.6.0**
The last versions proved to be very stable and with some additional sponsoring I was able to improve the DAM and TYPO3 workspaces support. Besides that a couple of issues which showed up in 4.4 and 4.5 were fixed. One of the next features will hopefully be a useful point of interest implementation - keep your fingers crossed that someone's clients want's to sponsor some time for that ;)

**Workspaces 4.5.1 / 4.5.2**
Even if it's shipped with the Core and included in the [official release notes](http://news.typo3.org/news/article/typo3-451-released/), here's my summary of the improvements. The workspace module itself brought us lot's of good feedback and also the new workspace preview raised some attention. Even though 4.5.0 was quite stable we weren't able to get it working perfectly. The fixes made for 4.5.1 made sure that especially the preview window works much more stable, it introduced state persistence (so switching preview modes or module settings are memorized properly) and it brought some performance improvements.
Btw. if you didn't check out the new workspaces features and improvements the[ new workspaces documentation from Susanne Moog](http://forge.typo3.org/projects/typo3v4-workspaces/repository/entry/workspaces/trunk/Documentation/manual.pdf) is a good point to start.
