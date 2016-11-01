---
author: tolleiv
comments: true
date: 2009-07-26T19:20:18Z
slug: typo3-extensions
tags:
- extension
- typo3
title: TYPO3 extensions ....
wordpress_id: 193
---

I'm currently within the preparation for some exams therefore my day's aren't very fancy at all - and that's why I just decided to introduce some TYPO3 extensions - especially TYPO3 extensions where I'm somehow involved ;)

The mentioned extensions are all created by AOEmedia or in cooperation with [AOEmedia](http://www.aoemedia.com), they're actively maintained and they work within regular TYPO3 environments with all TYPO3 features -lot's of other extensions don't support l10n and workspaces - ours do ;) Please use the mentioned pages on forge.typo3.org to get feedback on your bugs within the mentioned extensions.

Ok let's start:

![Imagemap Wizard preview](../wp-content/uploads/2009/07/39b125d4f5-197x300.png)**imagemap_wizard ([forge](http://forge.typo3.org/projects/show/extension-imagemap_wizard))**

My favorite extension - I started it on the T3DD08 and it took 6 months to finish the first alpha. After 1 year it's now a very flexible and fully integrated way to create imagemaps within TYPO3. I'm very proud that it comes without any XCLASSes and that it can just be used like regular content elements. Localization and workspaces are also supported and the imagemaps are created on frontend-like images. E.g. the uploaded image can be transformed with some installed filters and you'll see the applied filters also in the preview while creating your imagemap - that's pretty cool since there's no other imagemap-tool which offers such an integration. And the editing of the imagemap itself is done with a (hopefully) intuitive point&click interface :) ... more details on [forge.typo3.org](http://forge.typo3.org/projects/show/extension-imagemap_wizard).

**crawler ([forge](http://forge.typo3.org/projects/show/extension-crawler))**

The crawler is a widely know extension within the TYPO3 community - initially Kasper himself implemented the crawler and he handed over the maintenance for the crawler and it's associated extension staticpub to AOEmedia - during the last year we made a lot of small improvements and currently we're making really big changes to get multiprocess-support and  a better "configuration-interface" into the extension. The biggest task was that we had to stay compatible with the old API because it's also widely know within the community ;) ... a release of the new crawler version will follow very soon :) - more details about the crawler and [forge.typo3.org](http://forge.typo3.org/projects/show/extension-crawler).

**aoe_realurlpath ([forge](http://forge.typo3.org/projects/show/extension-aoe_realurl))**

RealURL is a powerful extension to generate readable and SEO friendly extensions within TYPO3, unfortunately RealURLs pathgenerations wasn't very flexible in the past and especially localization wasn't represented the way as you might expect it therefore [Daniel](http://www.typo3-media.com) implemented an extension which did the path-generations in a more predictable and flexible way. During the last months we added lot's of Unit tests and added lot's of additional feature for some other special cases (mostly connected to l10n) - the extension itself has a stable state and the testcases make development for this extension very easy - it's still a tricky job to implement new features though.

That's it for today - I think there'll be some additional posts on other extensions.

Followups: [Daniel about 10 extensions](http://www.typo3-media.com/blog/article/talking-about-10-extensions-part-i.html) , [popular exntensions on TYPO3.org](http://typo3.org/extensions/repository/popular/)
