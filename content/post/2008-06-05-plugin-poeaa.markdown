---
author: tolleiv
categories:
- creational
- factory
- pattern
- plugin
comments: true
date: 2008-06-05T01:11:00Z
slug: plugin-poeaa
tags:
- creational
- factory
- pattern
- plugin
title: Plugin [PoEAA]
wordpress_id: 572
---

I'm sure you're pretty often in the situation that you have to switch something depending on the context you're currently in. For example most people change their eating habits before summer - I also do :P

Sometimes this is what you also want to have in your software. To achieve different behaviour you normally just implement two different classes or aggregates and since they have the same "meaning" they normally share a interface (a.k.a Separated Interface). But who decides which implementation fits into the current environment/context?

The easiest way is to have a Factory Method with a small condition to decide this, but this method might grow very fast if you have various options. In this situation its also a matter of form to move this decision into some kind of configuration(-file) so that there's only on file which differs in various environments.

[Martin Fowler](http://martinfowler.com/eaaCatalog/plugin.html) suggests to place some kind of mapping into the configuration-file, so that your Factory knows which implementation to instantiate in the current context. The key for the mapping in this case is the name of the Separated Interface.

As you see in the example code below, there are Cookie Tins which create contain Cookies and depending on the current month you want to use a more or less restrictive Cookie Tin. In May, June and July you restrict your application to create max. 5 cookies, the rest of the year you don't care :P


    
    
    	// The Separated Interface
    interface CookieTin {
    	public function getCookieInstance();
    }
    
    class NormalTin implements CookieTin {
    	public function getCookieInstance() {
    		return new Cookie();
    	}
    }
    
    class DietTin implements CookieTin {
    	protected $i=0;
    	public function getCookieInstance() {
    		return ($this->i++ < 5)?new Cookie():new NullCookie();
    	}
    }
    
    // The Factory which supports Plugin-Creation
    class CookieTinFactory {
    	public static function getPlugin( $class ) {
    		try {
    			return new $GLOBALS['Plugins'][$class]();
    		} catch( Exception $e) {
    			// maybe call some default implementation if the mapping is wrong
    		}
    	}
    }
    




    
    
    class Cookie {
    	public function printCalories() {
    		echo ’200 ‘;
    	}
    }
    
    class NullCookie extends Cookie{
    	public function printCalories() { }
    }
    




    
    
    // This is usually in a configuration-file, normally this should
    // also sit in a XML structure….
    
    // That’s what it would be in 300 days/year
    $GLOBALS['Plugins']['CookieTin'] = “NormalTin”;
    // That’s what we have to turn it to before summer
    if(in_array(date(‘n’),array(5,6,7))) {
    	$GLOBALS['Plugins']['CookieTin'] = “DietTin”;
    }
    
    $tin = CookieTinFactory::getPlugin(‘CookieTin’);
    $i=10;
    while($i–) {
    	$cookie = $tin->getCookieInstance();
    	$cookie->printCalories();
    }
    



A very common use-case for this is the changed behaviour in a testing-environment compared to the production-environment.
There's something else in the example beside the Plugin Pattern - I also made use of the Special Case (the NullCookie) pattern  which I'm going to show in a future post....

