---
author: tolleiv
comments: true
date: 2011-12-22T17:23:16Z
slug: tagging-page-caches
tags:
- typo3
title: Tagging page caches
wordpress_id: 771
---

In our small TYPO3 world it's quite common to have list and single views for extensions on specific pages. That's quite nice because once a record is changed it allowes users to flush the caches for these pages automatically, using:

    
    TCEMAIN.clearCacheCmd=all|<pid>



But of course it isn't very smart to clear all caches once you have filled them up for hundred or even thousand records. To work around these limits it's quite handy to use an API which lives in [TYPO3 since 2008](http://git.typo3.org/TYPO3v4/Core.git?a=commit;h=03af46). It allows to add tags to the page cache and removes the caches by tag.

Adding tags to the current page can be done with this block:

    
    
    $GLOBALS['TSFE']->addCacheTags(array('tx_example_model:' . $id));
    



Removing the caches for that specific page could look like this:

    
    
    $GLOBALS['typo3CacheManager']->getCache('cache_pages')->flushByTag('tx_example_model:' . $id);
    



Correctly integrating this with your extension is quite easy. To set the tags, add the first block into your controller and make sure you provide the proper uid for the rendered domain objects. To remove the caches once a record is saved in the backend just register a t3lib_tcemain hook and flush the tagged caches. 

Credits: [Fabrizio Branca @fbrnc](http://twitter.com/fbrnc) - thx for pointing me to it ;)



* * *


The [Tcemain-Hook](http://blog.tolleiv.de/2010/03/handling-data-in-typo3-with-tcemain/) could be integrated with these two snippets:
_EXT:example/Classes/Hooks/Tcemain.php_

    
    
    class Tx_Example_Hooks_Tcemain {
    	public function processDatamap_afterDatabaseOperations($status, $table, $id, $fieldArray, &$reference) {
    		if ($table == 'tx_example_model') {
    			$GLOBALS['typo3CacheManager']->getCache('cache_pages')->flushByTag('tx_example_model:' . $id);
    		}
    	}
    }
    


_EXT:example/ext_localconf.php_

    
    
    $GLOBALS['TYPO3_CONF_VARS']['SC_OPTIONS']['t3lib/class.t3lib_tcemain.php']['processDatamapClass']['example'] = 'EXT:example/Classes/Hooks/Tcemain.php:Tx_Example_Hooks_Tcemain';
    




