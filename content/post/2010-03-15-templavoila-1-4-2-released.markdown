---
author: tolleiv
comments: true
date: 2010-03-15T23:39:53Z
slug: templavoila-1-4-2-released
tags:
- release
- templavoila
- typo3
title: TemplaVoila 1.4.2 released
wordpress_id: 375
---

Yesterday the 2nd team release of TemplaVoila was uploaded into the [TER](http://typo3.org/extensions/repository/view/templavoila/current/). It's basically a maintenance release which fixes more than 100 bugs. But since we haven't been that straight distinguishing between bug and usability feature, you'll see a couple of new things within this release.

The high level release notes are:



	
  * page module is now customizable with CSS and JavaScript

	
  * handling of static data structures are improved and fully working now

	
  * wizards are improved, new page wizard is more explaining

	
  * visual cleanups

	
  * new hooks for eTypes (elements added by mapping interface)

	
  * new classes for preview in page module, easy to override by extensions

	
  * added missing localisations

	
  * enhanced drag-and-drop in page module

	
  * over 100 Bugs are fixed

	
  * updated manual


During the installation your TYPO3 Extension Manager will ask to perform a couple of database upgrades. These upgrades aren't really critical because they just enlarge some database fields, which will make sure that your data really fit's in.

Just to point one thing out - especially the page module has been improved to be more flexible in certain parts.

These lines of TSConfig can be used to add CSS or JavaScript into the page module and enable easy customizations:

    
    mod.web_txtemplavoilaM1.stylesheet = ../fileadmin/css/tvpagemodule.css
    
    mod.web_txtemplavoilaM1.javascript {
      file1 = ../fileadmin/templates/js/jquery.js
      file2 = ../fileadmin/templates/css/backend.js
    }


 Further customizations is provided using the "**mod.web_txtemplavoilaM1.blindIcons**" configuration or with individual content preview classes (configured though "**$GLOBALS['TYPO3_CONF_VARS']['EXTCONF']['templavoila']['mod1']['renderPreviewContent']**" - see ext_localconf.php).

We haven't discussed what the direction for the next versions really looks like. From my perspective better TYPO3 integration, some kind of code cleanup and also the integration of some features which arise with TYPO3 4.4 will be somehow on our schedule. But since [TemplaVoila isn't a one man show anymore](http://blog.tolleiv.de/2010/03/re-farewell-templavoila/), this isn't just my decision and in addition I'd like to encourage everyone to send feedback, bugfixes or new features just to give us an impression what you like or dislike in the current version.

Last but not least, I'd like to thank everyone who was somehow involved in the release, especially Steffen Kamper who shared lot of inspiration and who has spent many hours to debug and fix some really tricky issues.
