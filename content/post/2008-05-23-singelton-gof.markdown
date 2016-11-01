---
author: tolleiv
categories:
- anti-pattern
- creational
- registry
- singleton
comments: true
date: 2008-05-23T21:40:00Z
slug: singelton-gof
tags:
- anti-pattern
- creational
- registry
- singleton
title: Singelton [GoF]
wordpress_id: 566
---

Tis pattern is a very well know and a often discussed one.

Explaining it the tasty way: let's say you have exactly one well know place where you store all your cookies and whenever you need one you can easy point to that place. Normally that's really comfortable because whatever you do with it, you don't have to search some place where you can find a cookie, you just go to that single "cookie tin" (or wherever you store cookies) and put a new cookie into it ;)

In modern OOP the singleton is not very popular, Eric Evans simple calls it anti-pattern. It makes it much harder to test your software and normally the user of the class, not the class itself, should know how many instances are needed.

But nevertheless for small applications it's very comfortable to use it in some situations and as long as you're also familiar with the drawbacks I think it's ok to consider using it....

So the example shows a cookie tin and how to use it and by the way it also implements some kind of lowlevel registry by using the [PHP5 magic methods](http://www.php.net/manual/en/language.oop5.magic.php), but that's another story ;)


    
    
    /**
    * CookieTin a.k.a Singleton object
    * we want to force our application
    * to hold exactly one CookieTin object
    */
    class CookieTin {
    	private $_cookies=array();
    
    	/* generic singleton part start*/
    
    	private static $_instance=NULL;
    	// prevent instatiation from the outside
    	private function __construct() { }
    
    	// prevent cloning
    	private function __clone() {}
    
    	public static function getInstance() {
    		if(!self::$_instance) {
    			self::$_instance = new self();
    		}
    		return self::$_instance;
    	}
    	/* generic singleton part end */
    
    	/* registry part start */
    	public static function __set($key,Cookie $value) {
    		echo “call setter\n”;
    		self::$_instance->_cookies[$key] = $value;
    	}
    
    	public static function __get($key) {
    		echo “call getter\n”;
    		return self::$_instance->_cookies[$key];
    	}
    
    	public static function __isset($key) {
    		echo “call isset\n”;
    		return isset(self::$_instance->_cookies[$key]);
    	}
    
    	public static function __unset($key) {
    		echo “call unset\n”;
    		unset(self::$_instance->_cookies[$key]);
    	}
    /* registry part end */
    }
    /**
    * Dummy Cookie
    */
    class Cookie {
    	public $name,$flavor;
    }
    




    
    
    $tin = CookieTin::getInstance();
    if(!isset($tin->granniescookie)) {
    	$tin->granniescookie = new Cookie();
    	$tin->granniescookie->name=“Grannies chocolate wonder”;
    	$tin->granniescookie->flavor=“chocolate”;
    }
    
    CookieTin::getInstance()->granniescookie->flavor = “chocolte chips”;
    
    var_dump($tin);
    unset(CookieTin::getInstance()->granniescookie);
    var_dump($tin);
    


The really important things happen in the last lines where the CookieTin is accessed statically and this change also (as you will see yourself) is reflected in the local instance if the $tin.
