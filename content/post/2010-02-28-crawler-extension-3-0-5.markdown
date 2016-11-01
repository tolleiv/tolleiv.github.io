---
author: tolleiv
comments: true
date: 2010-02-28T15:36:56Z
slug: crawler-extension-3-0-5
tags:
- crawler
- release
- typo3
title: crawler extension version 3.0.5 released
wordpress_id: 351
---

Quite some time after the 4.3 release of TYPO3, we published the necessary compatibility version of the crawler extension. Besides the compatibility fixes for TYPO3 and also for PHP 5.3 we also included some handy features:

First of all there's now, besides the CLI interface, also a full integration with the scheduler extension, which is available in TYPO3 4.3. This enables to setup crawler runs and manage all crawler releated tasks through the TYPO3 backend.

The "crawler_flush" interface was added to the CLI (and scheduler). It helps to clean up the crawler queue and enables to remove finished or unfinished entries.

In addition the CLI was cleaned up a little bit and behaves more intuitive in most situations. Also the help pages should now really tell you what options you have.

And last but not least we've added the possibility to avoid an additional HTTP request and have the crawler rendering the page directly.

Big kudos for their work and their support goes especially to Mick, Fabrizio and Timo from AOE media.
