---
author: tolleiv
comments: true
date: 2009-06-27T16:59:05Z
slug: disable-templavoila-new-page-wizard
tags:
- templavoila
- typo3
title: disable TemplaVoila new page wizard
wordpress_id: 158
---

[![templavoila-pagetemplate-selector](/uploads/2009/06/templavoila-pagetemplate-selector1.png)](/uploads/2009/06/templavoila-pagetemplate-selector1.png) TemplaVoila's "new page wizard" is a pretty cool featue - at least if your page templates have a thumbnail and a description. If your "new page wizard" look like the image on the right you might consider to turn it of.

That's pretty easy just paste this into the Page TSConfig of your rootpage and you're done:
`mod.web_list.newPageWiz.overrideWithExtension >`
Now you might wonder how TemplaVoila knows what template it should use to render the page - but due to the TV's default behaviour to inherit the page-template this should not be a problem if there's at least a TemplaVoila page template assigned for your root-page.

But honestly you should just upload a thumbnail and write some sentences about your page-templates - this should raise the user's happiness more than just disabling the wizard.
