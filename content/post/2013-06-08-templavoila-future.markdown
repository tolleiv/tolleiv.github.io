---
author: tolleiv
comments: false
date: 2013-06-08T07:20:34Z
slug: templavoila-future
title: TemplaVoila future
wordpress_id: 1133
---

If you followed some of my comments in the TYPO3 newsgroups recently, you've heard that I'm not very satisfied with the TYPO3 project in general and that's also reflected in my activity for TemplaVoila. After a certain time of inactivity I even had to ask myself whether it's wort to keep it in the TYPO3 universe or not. Due to the fact that this isn't an easy decision, I created a pro and con list which I'd like to share, before I make conclusions.


### Why TemplaVoila maintenance should continue:





	
  * it made TYPO3 attractive for many less technical people (people who don't even understand conditions or loops)

	
  * it contains and combines concepts (language, workspaces, content structuring) which aren't represented anyhow in other solutions

	
  * it is still used within the community and various indicators proof that it is still very popular




### Why TemplaVoila should not be maintained anymore:





	
  * it is not supported by the [active contributors](http://lists.typo3.org/pipermail/typo3-team-core/2013-April/053866.html) at all

	
  * it is constantly under some kind of PR-attack from the other solutions (which is very demotivating)

	
  * it lacks a developer "community" or at least a team

	
  * it has a horribly outdated documentation which has to be overworked

	
  * code refactoring is not really possible, the code is horrible to maintain

	
  * it's concepts can't be ported anyhow to FLOW/extbase (extbase itself is broken when it comes to workspaces or languages - no way to port over alternative concepts for these)

	
  * UI wise, Prototype and ExtJS have been used for it and need to be replaced with whatever the TYPO3 Core could offer

	
  * some of it's concepts need to be reworked (language) to be much more useable

	
  * the TYPO3 Core changes in a way that extension maintenance is no fun at all




### Conclusions:


I could add further points to both lists, but in general you'll get my point. All these have been on my mind for quite some time and I discussed them with various members of the TYPO3 community and I came to the conclusion that TemplaVoila should at least disappear from the TER to avoid that any new users start using it.

Along with that, I also came to the conclusion that handing over TemplaVoila back to Dmitry, Robert or Kasper wouldn't make sense either - the remaining workload is enormous and a single developer alone would never be able to deliver anything with reasonable quality (incl. documentation).

This basically means that TemplaVoila won't be actively distributed and supported by me and that there won't be any new public releases. In order to keep up the ability to fix bugs, I'd offer to keep [Forge+Git+Gerrit](http://forge.typo3.org/projects/extension-templavoila) open and I'm still willing to review and merge patches (through Gerrit). Even though I didn't see too much activity from others for TemplaVoila within Gerrit, I assume that this should be enough to support running projects.

To avoid that the discussion which might be needed for that announcement ends up in my blog, I'll close the comments here and I'd love to invite you to comment within the related newsgroup entry in [typo3.project.templavoila](http://lists.typo3.org/cgi-bin/mailman/listinfo/typo3-project-templavoila).
