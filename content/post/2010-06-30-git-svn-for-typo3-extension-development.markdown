---
author: tolleiv
comments: true
date: 2010-06-30T06:00:30Z
slug: git-svn-for-typo3-extension-development
tags:
- git
- typo3
- version control
title: Using git svn for TYPO3 extension development
wordpress_id: 391
---

When I first saw a presentation about Git 2 years ago I liked it, but I was convinced that Subversion covers all I need. Things have changed and especially the bad performance of Subversion for larger repositories and the need to commit things without messing up the official trunk motivated me to look up how to get started with Git. If you need more reasons why you should look into Git, you'll find them in [7] - anyways it's worth to have a look how I managed to work with it ;)

There are basically 2½ different tasks during the "daily" development.



	
  0. _Create a local working copy a.k.a clone the repository_
	
  1. Fix a bug or implement a feature and create a patch file with the changes

	
  2. Receive a patch and review it - eventually commit the changes





#### 0. Repository initialization


Getting started and creating the repository from an existing Subversion repository looks like this:

    
    mkdir templavoila
    cd templavoila
    git svn init -t tags -b branches -T trunk https://svn.typo3.org/TYPO3v4/Extensions/templavoila
    git-svn fetch

This will pull the entire SVN history into your local Git repository - use "git-svn fetch -r " to reduce the amount of imported revisions. 

To keep your repository up-to-date you need these commands:

    
    git stash
    git-svn fetch
    git rebase trunk
    git stash apply



If you followed the SVN trunk/tags/branches convention you should see that it also finds tags and branches during the import. But using "git branch" afterwards you'll see that there's only one local branch called "master". That's where Git shows it's strength the first time, because it distinguishes between local and remote branches. Work can only be done within local branches, whereas the existing SVN branches are only recognized as "remote" branches so far. To list all remote branches you can use "git branch -r". If you followed the trunk/tags/branches convention, you should see your SVN tags and branches within this list - otherwise you might want to read [8]. To make a remote (SVN) branch available in your local repository use:

    
    git checkout --track -b tv_1-4 templavoila_1-4






#### 1. Working with the repository


Starting to work on a new feature or a bug always starts with a new branch in git:

    
    git branch issue00012345
    git checkout issue00012345



You're now working in the "issue00012345" branch and all commits will just be committed to that branch. Once you made changes  you can either just commit all modifications with:

    
    git commit -a


or add particular files and commit only these files with:

    
    git add fileA
    git add fileB
    git commit


or with a nice pre commit preview which lets you decide which lines you want to commit:

    
    git add --patch
    git commit



**Preparing the patch for the mailingslist:**
In a pure Git workflow "git format-patch" is used to communicate patches, but due to the fact that there's a Subversion repository involved either the old style diff / patch or a workaround needs to be used to communicate changes.
Creating a patch which works for the mailinglist can basically be done using:

    
    git diff --no-prefix master > myPatchFile.patch


An alternative workaround to create proper SVN diff files (including svn revision etc..) can be found in [10] it's used very straight forward:

    
    git svn-diff > mySvnPatchFile.patch





#### 2. Review and commit patches


**Reviewing the patch someone sent to the list:**
As "usual" 
    
    patch -p0 < myPatchFile.patch

can be used to apply patch files.

Once the files passed the review the changes can be committed using "**git add**" and "**git commit**" as shown before.

**Committing to Subversion:**
Once you committed everything to your local repository you're able to perform this command to "forward" your commits to the Subversion repository. 

    
    git svn dcommit


Practically every single commit within git will also be reflected as a single Subversion commit. For several reason this might not always suit your situation.As a workaround you could merge all commits during the merge of your feature branches into the master branch using:

    
    git merge --commit --squash issue00012345
    git svn dcommit


Of course I don't recommend to do this for your regular work since have small granular commits is always better.



#### Sources and further reading


[1] [Patch-oriented development made sane with git-svn](http://spyced.blogspot.com/2009/06/patch-oriented-development-made-sane.html)
[2] [An introduction to git-svn for Subversion/SVK users and deserters](http://utsl.gen.nz/talks/git-svn/intro.html#using)
[3] [Git - SVN Crash Course](http://git-scm.org/course/svn.html)
[4] [Getting the GIT checkout](http://trac.parrot.org/parrot/wiki/git-svn-tutorial)
[5] [An Illustrated Guide to Git on Windows](http://nathanj.github.com/gitguide/tour.html)
[6] [git svn workflow - 
feature branches and merge](http://stackoverflow.com/questions/1129688/git-svn-workflow-feature-branches-and-merge)
[7] [whygitisbetterthanx.com](http://whygitisbetterthanx.com/#any-workflow)
[8] [Multiple branches using git-svn](http://www.dmo.ca/blog/20070608113513/)
[9] [Effectively using Git with subversion](http://www.viget.com/extend/effectively-using-git-with-subversion/)
[10] [Git Workflow with Upstream SVN](http://mojodna.net/2009/02/24/my-work-git-workflow.html)



#### Notes






  * Don't try to dcommit to https://svn.typo3.org/TYPO3v4/Extensions/templavoila unless you know what you're doing - I just used it to show you that it's even working with large history projects.

  * For projects hosted on forge.typo3.org there might be a native git support soon - stay tuned


  * Let me know if you had problems with any of the snippets


  * Further information about the review process for the TYPO3 Core and several other TYPO3 related projects can be found on [typo3.org](http://typo3.org/teams/core/resources/maintenance-policy/)



