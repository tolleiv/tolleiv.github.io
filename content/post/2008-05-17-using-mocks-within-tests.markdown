---
author: tolleiv
categories:
- mock
- phpunit
- tdd
comments: true
date: 2008-05-17T20:02:00Z
slug: using-mocks-within-tests
tags:
- php
- phpunit
- tests
title: Using Mocks within Tests
wordpress_id: 561
---

Let's say you have some kind of process which creates cookies and which requires the cookies to bake before you deliver them to the customers. This process somehow touches two kinds of objects the cookie itself and a object which performs the process - the cookie oven.
When you start to develop the oven how do you check whether the cookies are really baked or not? Since you can't get rid of the dependency between the oven and the cookie, you have to simulate a real object and do the checks by hand - but wait there's already a way to resolve it:
PHPUnit ships with a very nice and comfortable function to create and check mock-objects and that's exactly what we need in this situation where we somehow need to find out if our cookie really gets baked.
The testcase for this scenario could look like this:


    
    
    class TestCookieOven extends PHPUnit_Framework_TestCase {
    	public function testIfCookieIsBakenWithinFinishing() {
    		/* create the mock and expect the
    		bakeIt method to be called at least once */
    		$cookiemock = $this->getMock(‘Cookie’, array(‘bakeIt’));
    		$cookiemock->expects($this->once())->method(‘bakeIt’);
    
    		/* create the observed object and perform the required steps */
    		$cookieoven = new CookieOven();
    		$cookieoven->finishCookie($cookiemock);
    	}
    }
    


This first creates a mock and assigns the expectation that the "bakeIt"-function is called exactly once. [ beside the ->once()-call there are some alternatives: ->any(), ->never(), ->atLeastOnce(), ->exactly(int $count) and ->at(int $index) ]. Then it passes the mock to the oven and performs the method we want to check. To make this test green you need at least this:

    
    
    CookieOven {
    	/**
    	* Takes a cookie and prepares it to be ready
    	* for a customer
    	*
    	* @param Cookie $cookie
    	*/
    	public function finishCookie(Cookie $cookie) {
    		$cookie->bakeIt();
    	}
    }
    


So as soon you have these lines you have a green test and that's great because there is no need to have a real "cookie" class - someone else can take care of the cookies - oh wait ... I'd better do this myself :P
