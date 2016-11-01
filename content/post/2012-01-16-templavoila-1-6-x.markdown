---
author: tolleiv
comments: true
date: 2012-01-16T07:10:33Z
slug: templavoila-1-6-x
tags:
- templavoila
- typo3
title: TemplaVoila 1.6.x
wordpress_id: 815
---

Quite some things changed in the past months and I never found the time to clear up my mind and write up a summary.

First of all [TemplaVoila 1.6](http://typo3.org/extensions/repository/view/templavoila/current/) was released parallel to [TYPO3 4.6](http://typo3.org/download/release-notes/typo3-46/). It shipped with 22 bug and compatibility fixes. In general the 1.6.x branch is supposed to be compatible with TYPO3 4.4+ which also made it possible to clean up the extension quite a bit. In addition to that TemplaVoila 1.6.1 will show up in the TER in the next few hours. It fixes 10 additional issues.

Besides that, the main repository for TemplaVoila was moved to [git.typo3.org](git.typo3.org/TYPO3v4/Extensions/templavoila) and the old Subversion repository and my Github repository have been removed. The new repository location also enables and enforces a new way to contribute code changes for the project. Comparable to the TYPO3v4 Core every change request can be sent to Gerrit, where I can review the changes before they get merged into the repository. That workflow turns out to be very efficient for me. A detailed summary on how to contribute code to any repository hosted on [git.typo3.org](http://git.typo3.org) can be found on [wiki.typo3.org](http://wiki.typo3.org/Contribution_Walkthrough_Tutorials). To sum up some of the steps - here's how you submit a patch*:

    
    git clone git://git.typo3.org/TYPO3v4/Extensions/templavoila.git
    cd templavoila
    scp -p -P 29418 <username>@review.typo3.org:hooks/commit-msg .git/hooks/
    git checkout -b workingBranch
    # ... work on the files ...
    git add <changedFile>
    git commit
    git push origin HEAD:refs/for/master/<topic>


A further general change to the team happend more or less silently. During 2011 nobody from the old team or any new developer showed interest in the project and only few contributors helped with bugfixes or reviews. Due to that I also changed my attitude regarding future releases.
I'm currently planning to release one version parallel with every main TYPO3 4.x release, to make sure that TemplaVoila works in new versions and to make sure people can enjoy TYPO3 with TemplaVoila in the future as well. I'll also try to keep it compatible with all "stable" releases and aim to keep the issue count within the bugtracker as low as possible.
But I'm not planning to integrate new major features such as the field content sliding (use [EXT:kb_tv_cont_slide](http://typo3.org/extensions/repository/view/kb_tv_cont_slide/current/) please) or a context sensitive content wizard (a.k.a "content firewall"). Also major refactorings won't happen because they'd consume far too much of my time - therefore also the desperately needed update of the mapping module or a new new page module will not be implemented in the near future.

Nevertheless I'm open for any type of contribution and I'd be happy to review and test any patch showing up in Mantis or Gerrit - I'm just not able to spent a major part of my freetime for it.



* * *


* You have to sign the [Contributor License Agreement](http://typo3.org/about/licenses) to be able to push any change - it can be found on [typo3.org](http://typo3.org/about/licenses)
