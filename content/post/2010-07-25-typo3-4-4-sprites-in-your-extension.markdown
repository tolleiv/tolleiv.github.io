---
author: tolleiv
comments: true
date: 2010-07-25T08:00:29Z
slug: typo3-4-4-sprites-in-your-extension
tags:
- extension
- sprite
- typo3
title: TYPO3 4.4 sprites in your extension...
wordpress_id: 521
---

TYPO3 4.4 ships with a long list of improvements. One of them is the sprite API which was developed during T3UXW09. It extends the iconWorks API and enables to retrieve any backend-icon from a central sprite.

As extension maintainer there are several ways to use these new parts of the API you can either just use it to display core icons, you can include your own icons and retrieve it with the new API or you can include your own sprite and retrieve the separate icons.



#### Use sprite icons


Using sprite icons is pretty easy, to get a checkmark icon you could use this:

    
    t3lib_iconWorks::getSpriteIcon('status-dialog-ok');


The list of all available icons can be retrieved using the '**[spriteiconoverview](http://typo3.org/extensions/repository/view/spriteiconoverview/current/)**' extension which I just recently pushed into the TER ;)



#### Add your own icons


The API also provides the possibility to include your extension's icons using either single icons or your own icon sprite. 
Using single icons can be done with the following code in your ext_tables.php or ext_localconf.php:

    
    
    $icons = array(
         'myicon' => t3lib_extMgm::extRelPath('myextension') . 'myicon.gif'
    );
    t3lib_SpriteManager::addSingleIcons($icons, 'myextension');
    


Using this icon is easy again:

    
    t3lib_iconWorks::getSpriteIcon('extensions-myextension-myicon');



If your extension comes with a larger amount of icons you might want to use your own sprite. This includes your sprite as a image file and a CSS file. The css file should look like this:

    
    
    .t3-icon-extensions-myextension {
         background-image:url(../../typo3conf/ext/myextension/myextension_sprite.gif);
    }
    .t3-icon-extensions-myextension-icon1 {	background-position: 0px 0px; }
    .t3-icon-extensions-myextension-icon2 {	background-position: 0px -16px; }
    

The path to your sprite needs to be relative to the typo3temp/sprites/ folder, from where the (temporary) merged CSS file will be included.
This sprite can be included using:

    
    
    $icons = array(
         'extensions-myextension-icon1',
         'extensions-myextension-icon2'
    );
    t3lib_SpriteManager::addIconSprite(
         $icons,
         t3lib_extMgm::siteRelPath('myextension') . 'myextension_sprite.css'
    );				
    


As you see it's quite easy to include the new API, the API provides some further options for overlays and further modifications which I didn't mention here. And as a final motivation this is a comparison between old and new API to get a "back" button:

    
    
    // old API
    $icon = '<img' . t3lib_iconWorks::skinImg($this->doc->backPath,'gfx/goback.gif','width="14" height="14"') . 'alt="" />';
    
    // new API
    $icon = t3lib_iconWorks::getSpriteIcon('actions-view-go-back');
    


----
The sprite API is quite new and my knowledge about how to use it is also relatively fresh - so please let me know if you've any remarks or questions.
Btw. special kudos for his involvement during the implementation of this nice feature and many many thanks for helping me to understand it, to [Steffen Ritter](http://www.rs-websystems.de/)  :)
----
Further reading:



	
  * If you've never heard of sprites you might want to read this article an [alistapart.com](http://www.alistapart.com/articles/sprites)

	
  * [TYPO3 4.4 release notes](http://typo3.org/download/release-notes/typo3-44/)

	
  * [news.typo3.org - T3DD10: It's all about sprites](http://news.typo3.org/news/article/t3dd10-its-all-about-sprites/)

	
  * [spriteiconoverview](http://typo3.org/extensions/repository/view/spriteiconoverview/current/) extension



----




