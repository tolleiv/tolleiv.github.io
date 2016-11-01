---
author: tolleiv
comments: true
date: 2010-03-02T07:05:16Z
slug: handling-data-in-typo3-with-tcemain
tags:
- extension
- typo3
title: Handling data in TYPO3 with tcemain
wordpress_id: 312
---

TYPO3 is (by definition) a powerful tool when it comes to data. Besides creating, updating and deleting data there are also localizing and versioning, logging and even rollbacks. All this is provided through the GUI of TYPO3 and all the technical stuff in working under the hood of TYPO3 for nearly every piece of data. But what if you're asked to write a script which imports or updates data, how can you make sure that all this is done in a TYPO3 compatible way?

The lazy programmers approach is to write up SQL, but that's not what's recommended if you still want the full TYPO3 featureset to be available for you (without reinventing the wheel). In this case the TYPO3 core class tslib_tcemain (short tcemain) is what you're looking for. For the mentioned tasks there are two main functions relevant - **process_cmdmap()** and **process_datamap()**. The **process_cmdmap()** performs actions like "move", "copy", "localize", "version" (create, stage, swap, flush), "delete" and "undelete". The **process_datamap()** does the rest - creating records, updating datafields. Controlling both of them is done with configuration arrays and that's how it looks like**:

**Creating a record*: **

    
    $data = array();
    $data['tt_content']['NEW'] = array(
    	'pid' => 100,
    	'header' => 'A new thing'
    );
    
    $tce = t3lib_div::makeInstance ('t3lib_TCEmain');
    $tce->start ($data, array());
    $tce->process_datamap ();
    
    echo "The new element has the uid ".$tce->substNEWwithIDs['NEW'];
    



Creates a new tt_content record on page 100 with the header set to "A new thing". 

**Updating data*:**

    
    $data = array();
    $data['tt_content']['110'] = array(
    	'header' => 'A really new thing'
    );
    
    $tce = t3lib_div::makeInstance ('t3lib_TCEmain');
    $tce->start ($data, array());
    $tce->process_datamap ();



Updates the header field of the content element with the uid 110 to "A really new thing".

**Move data from one page to another*:**

    
    $cmd = array();
    $cmd['tt_content']['110']['move'] = 101;
    
    $tce = t3lib_div::makeInstance ('t3lib_TCEmain');
    $tce->start (array(), $cmd);
    $tce->process_cmdmap ();



Moves the tt_content record with the uid 110 to the page 101.

**Copy data from one page to another*:**

    
    $cmd = array();
    $cmd['tt_content']['110']['copy'] = 101;
    
    $tce = t3lib_div::makeInstance ('t3lib_TCEmain');
    $tce->start (array(), $cmd);
    $tce->process_cmdmap ();



Copythe tt_content record with the uid 110 to the page 101.

**Localize your record*:**

    
    $cmd = array();
    $cmd['tt_content']['110']['localize'] = 5;
    
    $tce = t3lib_div::makeInstance ('t3lib_TCEmain');
    $tce->start (array(), $cmd);
    $tce->process_cmdmap ();



This creates a localization for the language 5 of the tt_content record with uid 110 (assuming that the tt_content record 110 is a default language record).

**Delete*:**

    
    $cmd = array();
    $cmd['tt_content']['110']['delete'] = true;
    
    $tce = t3lib_div::makeInstance ('t3lib_TCEmain');
    $tce->start (array(), $cmd);
    $tce->process_cmdmap ();



Deletes the tt_content record with the uid 110.

**Undelete*:**

    
    $cmd = array();
    $cmd['tt_content']['110']['undelete'] = true;
    
    $tce = t3lib_div::makeInstance ('t3lib_TCEmain');
    $tce->start (array(), $cmd);
    $tce->process_cmdmap ();



Restores the tt_content record with the uid 110 - if it's deleted.

----

* Running the codes requires a TYPO3 backend context with a logged in backend user who has the right to perform all these actions. In addition tcemain has some configuration options to change the behaviour of the actions, e.g. "$enableLogging" or "$bypassWorkspaceRestrictions" - they come with useful defaults but you might need to change them in certain situations ~ so looking into the code documentation might save you some time.

**  I left out the "version" part since this requires some more explanation than just a few lines of code.
