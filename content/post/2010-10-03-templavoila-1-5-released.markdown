---
alias:
- /2010/10/templavoila-1-5-released/,
author: tolleiv
comments: true
date: 2010-10-03T20:31:33Z
slug: templavoila-1-5-released
tags:
- templavoila
- typo3
title: TemplaVoila 1.5 released
wordpress_id: 699
---

The new version comes with many bugfixes,new features and a closer TYPO3 integration. Overall 95 issues have been resolved in the last 4 months to finalize this versions, some of the highlights are:



#### HTML5 support


The full list of HTML5 tags is now supported in TemplaVoila. The restrictions to specific tags was removed and the TYPO3 integrator is now able to use the full bandwidth of modern HTML. With this change also the tag-icons themself were replaced and the coloring schema was changed. The inspiration for the current color schema came from [Josh Duck's "Periodic Table of the Elements"](http://joshduck.com/periodic-table.html). In additon same mapping bugs have been resolved too - for details see [#13974](http://bugs.typo3.org/view.php?id=13974) and [#14881](http://bugs.typo3.org/view.php?id=14881).

[![TemplaVoila 1.5 page module](/uploads/2010/09/tv15-page-module-300x190.png)](/uploads/2010/09/tv15-page-module.png)



#### TYPO3 4.4 Look&Feel and docHeader integration


TYPO3 4.4 introduced a new skin and changed the look&feel in the backend radically. Once installed in 4.4 TemplaVoila 1.5 adjusts it's look and provides the same usability improvements as the official page module. The page-module was optimized to use as much "official" CSS as possible to support designes with their own backend-skins. In addition to the CSS&Markup changes, TemplaVoila also uses the [TYPO3 4.4 SpriteIcon API](http://blog.tolleiv.de/2010/07/typo3-4-4-sprites-in-your-extension/) to provide and retrieve backend icons and uses the FlashMessage API to style all backend notifications.

Another important step was the integration of the so called docHeader. This is the area at the very top of each backend module page which provides useful tools and action-icons. With this version TemplaVoila finally provides docHeaders within every backend-part.



#### Improved TYPO3 integration


Besides the visual changes the general TYPO3 integration has been improved with various modifications.

With the current version there's no need to give "Edit page" rights to you editors if they want to add or remove content elements. Just the "Edit content" right and access to the "Page>Content" field is enough for them. For details see: [#3903](http://bugs.typo3.org/view.php?id=3903)

The "advached header link inclusion" is one of the integration steps in the frontend. All resources which are related to an FCE are passed through an TYPO3 API (pageRenderer). This avoids duplicate inclusion of one resource (e.g. CSS files) and enables further post-processing (e.g. compression or merging). It can be enabled using the "advancedHeaderInclusion" within your TypoScript setup which could then look like this:


    page = PAGE
    page.typeNum = 0
    page.10 = USER
    page.10.userFunc = tx_templavoila_pi1->main_page
    page.10.advancedHeaderInclusion= 1




---
The full list of changes within this version can be found on [bugs.typo3.org](http://bugs.typo3.org/search.php?project_id=2&sticky_issues=on&target_version=1.5.0&sortby=last_updated&dir=DESC&hide_status_id=90).
Many many thanks to all contributors and reviewers - it's great that more people try to help out and it keeps me motivated to continue improving this great TYPO3 extension.

