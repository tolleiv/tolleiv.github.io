---
author: tolleiv
categories:
- behavioral
- iterator
- pattern
- php
- spl
comments: true
date: 2008-07-07T17:30:00Z
slug: iterator-gof
tags:
- behavioral
- iterator
- pattern
- php
- spl
title: Iterator [GoF]
wordpress_id: 585
---

Lot's of people like to write down things into lists, so that they can go through that list later and check whether everything was fine. Normally every recipe has a list ~ there's always a list of ingredients at the beginning of the recipe.
This short example shows how such a list can be processed in PHP. So why would you want to have something else than a array to hold your objects? - My example still uses a array to hold the objects (uni- or bidirectional lists would also be possible) but it adds a kind of a facade to the array so that the common managements-tasks are handled within the List-Object. Everything you need for this example is present in PHP since version 5.0. The basic steps you need to do is to provide a "Object" and an "ObjectList" which implements the native ["Iterator"](http://www.php.net/manual/en/language.oop5.iterations.php) interface and then you're able to have very handy lists :)





class Incredient {

public $name,$amount;
public function __construct($name,$amount) {
$this->name = $name;
$this->amount = $amount;
}
}

class Recipe implements Iterator {
public $title;
private $ingredients;

public function __construct ($title) {
$this->title = $title;
}

public function addIncredient(Incredient $in) {
$this->ingredients[] = $in;
}

public function current ()  {   return current ($this->ingredients);    }
public function key ()      {   return key($this->ingredients);         }
public function valid ()    {   return current ($this->ingredients);    }
public function rewind ()   {   return reset ($this->ingredients);      }
public function next ()     {   return next ($this->ingredients);       }
}










$cookieRecipe = new Recipe("Chocolate Cookie");
$cookieRecipe->addIncredient(new Incredient('Flour','2.5 cups'));
$cookieRecipe->addIncredient(new Incredient('Baking soda','1 teaspoon'));
$cookieRecipe->addIncredient(new Incredient('Salt','0.5 teaspoon'));
$cookieRecipe->addIncredient(new Incredient('Butter','1 cup'));
$cookieRecipe->addIncredient(new Incredient('Sugar','1 cup'));
$cookieRecipe->addIncredient(new Incredient('Brown Sugar','0.5 cup'));
$cookieRecipe->addIncredient(new Incredient('Vanilla extract','1 teaspoon'));
$cookieRecipe->addIncredient(new Incredient('Egg','1-2'));
$cookieRecipe->addIncredient(new Incredient('Chocolate chips','2 cups'));

// process recipe:
foreach($cookieRecipe as $inc) {
echo $inc->name." => ".$inc->amount."<br/>";
}




As you see it's pretty easy to have lists of objects in PHP. You might also think that always creating to some list-object over and over again is very odd and you're right. For the most common tasks like [iterating through arrays](http://www.php.net/manual/en/class.arrayiterator.php), [directory-lists](http://www.php.net/manual/en/class.directoryiterator.php) and a few more task you can use objects which are shipped with the Standard PHP Library ,which is also part of PHP since version 5 and mandatory in 5.3. So the example shown above could also look like this:




$recipe = array();
$recipe[] = new Incredient('Flour','2.5 cups');
$recipe[] = new Incredient('Baking soda','1 teaspoon');
$recipe[] = new Incredient('Salt','0.5 teaspoon');
$recipe[] = new Incredient('Butter','1 cup');
$recipe[] = new Incredient('Sugar','1 cup');
$recipe[] = new Incredient('Brown Sugar','0.5 cup');
$recipe[] = new Incredient('Vanilla extract','1 teaspoon');
$recipe[] = new Incredient('Egg','1-2');
$recipe[] = new Incredient('Chocolate chips','2 cups');

$recipeIncObj = new ArrayObject($recipe);
$ingredientsIt = $recipeIncObj->getIterator(); 

foreach($ingredientsIt as $inc) {
echo $inc->name." => ".$inc->amount."<br/>";
} 




As I said at the beginning, there are lots of situations where you might want to have a list for something and if you store that list in PHP the Iterator-pattern can keep your code clean and tasty :)
