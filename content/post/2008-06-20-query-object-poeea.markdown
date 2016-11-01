---
author: tolleiv
categories:
- creational
- database
- factory
- pattern
- query
- specification
comments: true
date: 2008-06-20T17:29:00Z
slug: query-object-poeea
tags:
- creational
- database
- factory
- pattern
- query
- specification
title: Query Object [PoEEA]
wordpress_id: 581
---

Maybe you remember the [Specification Pattern](http://www.cookiepattern.com/2008/05/specification-ddd.html) I explained some weeks ago. It enabled a easy and intuitive  searching within large object-collections. A drawback of my example was that I stored the objects in the memory. This can be really ineffective if you want a single object out of hundreds, because you have to create all of them to see which one fits the specification.
Normally you want to limit the number of objects and you also don't store large datasets in memory. The idea of the Query Object pattern is that it enables a usage, comparable to the specification pattern, for objects which are persisted in a database. The benefit is that it creates a query to exclude objects which won't satisfy your needs and therefore you wont mess up the memory anymore.
Once you have a query-object in place you should not get in touch with SQL anymore because it can encapsulate SQL completely, at least if you also have some kind of data mapping (coming soon), which is a great benefit for everyone who is not so familiar with SQL. (But don't forget, regarding performance, SQL-optimization is a very important thing).

So what we need for the Query Object in first place is a object, I'll use the Cookie out of the [specification pattern post](http://www.cookiepattern.com/2008/05/specification-ddd.html) again. Then we need criteria-objects which hold the information for a single criteria, (determined by "database-field", "operator" and "value") and we also need the Query Objects itself to wrap up the SQL-querying and the object creation somehow.

A very simple example could look like this:




interface Critera {
public function getWhereClause();
}

class CookieCriteria implements Critera {

private $operator,$field,$value;

protected function __construct($operator,$field,$value) {
$this->operator=$operator;
$this->field=$field;
$this->value=$value;
}

public function getWhereClause() {
return implode(" ",array($this->field,$this->operator,$this->value));
}

public static function matches($field,$value) {
return new CookieCriteria("LIKE",$field,'"'.$value.'"');
}

public static function greaterThan($field,$value) {
return new CookieCriteria(">",$field,$value);
}
}

class CookieFinder {
protected $criterias;

public function addCriteria(Critera $criteria) {
$this->criterias[] = $criteria;
}

public function generateSQL() {
$sql = "SELECT * FROM cookies";

if(sizeof($this->criterias)) {
$where=array();
reset($this->criterias);
while(list(,$criteria)=each($this->criterias)) {
$where[] = $criteria->getWhereClause();
}
$sql.= sizeof($where)?" WHERE ".implode(" AND ",$where):"";
}
return $sql;
}

public function find() {
$collection = array();
if(!$result = mysql_query($this->generateSQL())) {
throw new Exception(mysql_errno());
}
while($row = mysql_fetch_assoc($result)) {
$collection[] = new Cookie($row['name'],$row['flavor'],$name['size']);
}
return $collection;
}
}




Possible client code could look like this:
$finder = new CookieFinder();
$finder->addCriteria(CookieCriteria::matches("name","Granny%"));
$finder->addCriteria(CookieCriteria::greaterThan("size",100));
$cookies = $finder->find();

We just pick up the Query Object, add one or more criteria and ask it to create the objects which fit them.

So this example is not as powerful as the one I used for the [Specification pattern](http://www.cookiepattern.com/2008/05/specification-ddd.html), but it should be a easy task to create some kind of "nested criteria objects".
Query objects normally make use of data-mapping so that you can handle various classes, stored in different tables/databases, with a single and generic Query Object. This also enables to avoid SQL-Injection, since you're able to validate the fields and values before you sent them to your database, also some kind of database abstraction would be possible.
With the "Query Object by example", which requires to build up a single object which is used as blueprint for the required objects, exists another flavor of this pattern which is very handy to use and more descriptive.
But no matter which flavor you prefer, Query Objects bring some real benefits when you've to handle complex datasets - for smaller projects the effort might be to much so be careful where you use it.
