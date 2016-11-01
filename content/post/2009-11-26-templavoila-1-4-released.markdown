---
author: tolleiv
comments: true
date: 2009-11-26T20:00:19Z
slug: templavoila-1-4-released
tags:
- release
- templavoila
- typo3
title: TemplaVoila 1.4 released...
wordpress_id: 269
---

A new version of  TemplaVoila ([ter](http://typo3.org/extensions/repository/view/templavoila/current/)) ([forge](http://forge.typo3.org/projects/show/extension-templavoila)) has just been released. Besides the great fact that this is the first **team-release** of TemplaVoila, the high-level improvements within the pagemodule (drag'n'drop),  the mapping module and the FCE editing forms and besides lot's of bug fixes, there are a few very nice features which make the day-to-day work with TemplaVoila much easier.

The list of things which happend in 1.4 can be seen on bugs.typo3.org ([preselected filter](http://tinyurl.com/yb9wsnk)). The following list contain my favorite fixes which happened unter the hood:

**1) Delete content within the pagemodule (instead of unlinking)**
By default you will still see the "unlink" icon within the pagemodule but one small setting within User- or PageTSconfig will show up delete buttons as well. There are two modes:
_mod.web_txtemplavoilaM1.enableDeleteIconForLocalElements = 1 _
will show unlink and delete icons for local elements side-by-side
_mod.web_txtemplavoilaM1.enableDeleteIconForLocalElements = 2 _
will show the delete icon and hide the unlink icon whenever possible.

Details: [http://bugs.typo3.org/view.php?id=6869](http://bugs.typo3.org/view.php?id=6869)

**2) Skip edit screen after a new content element was created**
Especially for container items it's anoying that the TYPO3 edit screen opens up after such an item was created. The setting _noEditOnCreation_** **within the meta configuration part of your datastructure can be used to change that.

Details: [http://bugs.typo3.org/view.php?id=8079](http://bugs.typo3.org/view.php?id=8079)

**3) Hide TemplaVoila field values and cleanup the pagemodule
**Another problem within large projects is a messed up pagemodule. Very often the field data of flexible content elements shows up without any chance to hide it. Use  _disableDataPreview** **_within the meta configuration part of your datastructure to change that.

Details: [http://bugs.typo3.org/view.php?id=11520](http://bugs.typo3.org/view.php?id=11520)

**4) Define default record values**
A proper setup contains good default values. Within datastructures you can define default values for your flexform-fields and from now on TemplaVoila also provides the possibility to define default-values for the fields of the parent record. The "default / TCEForms / <fieldname>" parts within the meta configuration part of your datastructure does this. Very useful usage for container elemente is:

<meta type="array">
<langDisable>1</langDisable>
<default>
<TCEForms>
<sys_language_uid>-1</sys_language_uid>
</TCEForms>
</default>
</meta>

Details: [http://bugs.typo3.org/view.php?id=8759](http://bugs.typo3.org/view.php?id=8759)

There's also a completly new "New content element"-wizard which can be configured with PageTSConfig - this wizard brings some additional feature for default-value configuration. I'm going to bring that up in another post soon.

**5) sectionCount / sectionPos register in TypoScript
**Rending fields which are nested in sections is not always fun. Expecially when it comes to position detection for the current item. Two new registers try to help in such situtations:

tx_templavoila_pi1.sectionCount holds the overall amount of items within the current section
tx_templavoila_pi1.sectionPos holds the position of the curren titem -starting with 1

Example:
10 = TEXT
10.value = (last item)
10.if.equals.data = register:tx_templavoila_pi1.sectionPos
10.if.value.data = register:tx_templavoila_pi1.sectionCount

Details: [http://bugs.typo3.org/view.php?id=7263](http://bugs.typo3.org/view.php?id=7263)




    
    noEditOnCreation



