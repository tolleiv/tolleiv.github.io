---
author: tolleiv
categories:
- pattern
- service
- specification
comments: true
date: 2008-05-15T14:00:00Z
slug: specification-ddd
tags:
- pattern
- service
- specification
title: Specification [DDD]
wordpress_id: 560
---

Maybe sometimes the cookie tin is filled with all sorts of cookies and only some of them are what you'd like in this special moment. Often it's very easy to specify which cookie you like, but sometimes your wishes are very complex, for example if you look for grannies special cookie with chocolate, coconut and vanilla crumbles. This could lead into a real crumby problem if you try to sort all the cookies and then select eat the right one.

In the world of OOP it's often much harder to collect a few objects out of a large number of different objects, also combining different requirements isn't easy and that's where the specification pattern can help you out. It implements some basic operations to combine requirements (AND, OR, NOT) and the only thing a concrete specification for a concrete class has to do is to implement a method (isSatisfied()) which is able to determine whether a object meats a requirement or not.

I splitted the generic part of the script and the cookie-example. The first part just implements the methods which are needed for the combination and provides a abstract class which is extended by the specifications in the second part. As you see the specification is combined which normally encapsulates the retrieval of the objects ,for example from a database. ....just have a look it's really easy to select the right cookie ... yummy


[![](http://bp2.blogger.com/_l5fIZzJyYfc/SC115dPibfI/AAAAAAAAABQ/ZZfCsWJAwDA/s400/specification_sequenze.png)](http://bp2.blogger.com/_l5fIZzJyYfc/SC115dPibfI/AAAAAAAAABQ/ZZfCsWJAwDA/s1600-h/specification_sequenze.png)




    
    
    interface Specification {
    	public function isSatisfiedBy($obj);
    	public function _and(Specification $spec);
    	public function _or(Specification $spec);
    	public function _not();
    }
    
    abstract class AbstractSpecification implements Specification {
    	public function isSatisfiedBy($obj) { }
    	public function _and(Specification $spec) {
    		return new AndSpecification($this, $spec);
    	}
    	public function _or(Specification $spec) {
    		return new OrSpecification($this, $spec);
    	}
    	public function _not() {
    		return new NotSpecification($this);
    	}
    }
    
    class AndSpecification extends AbstractSpecification {
    	private $spec1, $spec2;
    	public function __construct(Specification $spec1, Specification $spec2) {
    		$this->spec1 = $spec1;
    		$this->spec2 = $spec2;
    	}
    	public function isSatisfiedBy($obj) {
    		return $this->spec1->isSatisfiedBy($obj) && $this->spec2->isSatisfiedBy($obj);
    	}
    }
    
    class OrSpecification extends AbstractSpecification {
    	private $spec1, $spec2;
    	public function __construct(Specification $spec1, Specification $spec2) {
    		$this->spec1 = $spec1;
    		$this->spec2 = $spec2;
    	}
    	public function isSatisfiedBy($obj) {
    		return $this->spec1->isSatisfiedBy($obj) || $this->spec2->isSatisfiedBy($obj);
    	}
    }
    
    class NotSpecification extends AbstractSpecification {
    	private $spec;
    	public function __construct(Specification $spec) {
    		$this->spec = $spec;
    	}
    	public function isSatisfiedBy($obj) {
    		return !$this->spec->isSatisfiedBy($obj);
    	}
    }
    



    
    
    /**
    * Cookie object just a container for the relevant data.
    *
    */
    class Cookie {
    	protected $name,$flavor,$size;
    	public function __construct($name=”,$flavor=‘chocolate’,$size=100) {
    	$this->name=$name;
    	$this->flavor = $flavor;
    	$this->size = abs($size); // avoid negative size
    	}
    	public function getName() { return $this->name; }
    	public function getFlavor() { return $this->flavor; }
    	public function getSize() { return $this->size; }
    }
    /**
    * Cookie service delivers cookies, offers some ways to select specific types of cookies
    *
    */
    class CookieService {
    	protected $cookies = array();
    	/**
    	* Add a cookie to the collection
    	* name is used as identifier, thats not the best choice
    	* but it’s ok for the example
    	*
    	* @param Cookie $cookie
    	*/
    	public function add(Cookie $cookie) {
    		$this->cookies[$cookie->getName()]=$cookie;
    	}
    	/**
    	* Generic method to check which objects fit the spec
    	*
    	* @param Specification $spec
    	*/
    	private function filter(Specification $spec) {
    		$result=array();
    		reset($this->cookies);
    		foreach($this->cookies as $name=>$cookie) {
    			if($spec->isSatisfiedBy($cookie)) {
    				$result[]=$cookie;
    			}
    		}
    		return $result;
    	}
    
    	public function getLargeCookies() {
    		$spec = new SmallCookieSpecification();
    		$spec = $spec->_not();
    		return $this->filter($spec);
    	}
    	public function getSmallChocolateCookies() {
    		$spec = new SmallCookieSpecification();
    		$spec = $spec->_and(new ChocolateCookieSpecification());
    		return $this->filter($spec);
    	}
    	public function loadDummyData() {
    		$this->add(new Cookie(‘Granny\’s classic’,‘chocolate’,60));
    		$this->add(new Cookie(‘Modern Jumbo’,‘moca,chocolate’,180));
    		$this->add(new Cookie(‘Kitchen Sink’,‘macadamia,cranberrie’,90));
    		$this->add(new Cookie(‘Vanilla Cloud’,‘vanilla’,120));
    		$this->add(new Cookie(‘Chocolate chip’,‘coconut,chocolate’,160));
    	}
    }
    
    class SmallCookieSpecification extends AbstractSpecification {
    	public function isSatisfiedBy($obj) {
    		return $obj->getSize() < 100;
    	}
    }
    
    class ChocolateCookieSpecification extends AbstractSpecification {
    	public function isSatisfiedBy($obj) {
    		return stristr(strtolower($obj->getFlavor()),‘chocolate’) !== FALSE;
    	}
    }
    
    /**
    * Client code
    */
    $service = new CookieService();
    $service->loadDummyData();
    echo ‘
    
    ’;
    var_dump($service->getLargeCookies());
    var_dump($service->getSmallChocolateCookies());
    echo ‘

’;




[more information by E.Evans and M.Fowler](http://martinfowler.com/apsupp/spec.pdf)
