---
author: tolleiv
categories:
- Coding
- pattern
- typo3
comments: true
date: 2011-11-30T17:17:02Z
slug: signal-slots-in-extbase
tags:
- extbase
- pattern
- typo3
title: Signal / Slots in Extbase
wordpress_id: 758
---

A nice thing to have at hand is definately [Signal and Slots](http://en.wikipedia.org/wiki/Signals_and_slots). I heard [Felix](http://f.oer.tel) talking about them quite often and I finally found a nice usecase and came to play with them a little bit this afternoon. And just to avoid that others have to look around too much to find how they can get them to work here's how it's working for me.

First of all you should understand the concept. This nice little "definition" (from [flow3.typo3.org](http://flow3.typo3.org/documentation/guide/partiii/signalsandslots.html)) sums it up pretty well:


> A signal, which contains event information as it makes sense in the case at hand, can be emitted (sent) by any part of the code and is received by one or more slots, which can be any function<del> in FLOW3</del> in extbase.



To get this running in extbase, you've to get hold of the _Tx_Extbase_SignalSlot_Dispatcher_, which is the central instance to manage all of it. Within Extbase that's done easily with this snippet within your classes:

    
    
       ...
    	/**
    	 * @var Tx_Extbase_SignalSlot_Dispatcher
    	 */
    	protected $signalSlotDispatcher;
       
    	/**
    	 * @param Tx_Extbase_SignalSlot_Dispatcher $signalSlotDispatcher
    	 */
    	public function injectSignalSlotDispatcher(Tx_Extbase_SignalSlot_Dispatcher $signalSlotDispatcher) {
    		$this->signalSlotDispatcher = $signalSlotDispatcher;
    	}
       ...
    



Next thing is to make use of it. The Slot (listener) part could look like one of following blocks. In all cases you define the Signal by it's class (not necessarily a PHP Class) and it's name. Next to that the Slot can either be defined by a Closure, an object with a method name or a PHP-Class and a method name.

    
    
       ...
    // Using a closure
    $this->signalSlotDispatcher->connect(
          'Crunching', 'emitDataReady', function($data) { crunch($data) }, NULL, FALSE
    );
       ...
    // Using a method of the current object
    $this->signalSlotDispatcher->connect(
         'Crunching', 'emitDataReady', $this, 'crunch', FALSE
    );
       ...
    // Using a method of the specified class
    $this->signalSlotDispatcher->connect(
         'Crunching', 'emitDataReady', 'Cruncher', 'crunch', FALSE
    );
       ...
    



To trigger the Signal which invokes the Slots registered above, you've to run the following code.

    
    
    $this->signalSlotDispatcher->dispatch('Crunching', 'emitDataReady', array($data));
    



One thing I found was that by default the _Tx_Extbase_SignalSlot_Dispatcher_ it not a Singleton in older extbase versions. Bastian fixed that already in the [master](https://review.typo3.org/#change,6785) and [1-4](https://review.typo3.org/#change,6786) branches and lucky enough this change was part within the TYPO3 4.6.1 release. But I think it's still important to mention that this wasn't the default from the beginning on.

Even if [AOP is a nicer way to implement this feature](http://flow3.typo3.org/documentation/guide/partiii/signalsandslots.html), the extbase backport still works pretty straigh forward.

Edit: One thing I've to add - Felix is not "just" talking about Signal/Slots - he's the one to thank for the [backport](https://review.typo3.org/1563). And now that his [blog](http://blog.foertel.com/2011/10/using-signalslots-in-extbase/) is running again - this post seems like a summary ;) 
