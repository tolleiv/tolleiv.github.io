---
author: tolleiv
categories:
- behavioral
- pattern
- php
- value object
comments: true
date: 2008-05-30T04:39:00Z
slug: value-object-ddd
tags:
- behavioral
- pattern
- php
- value object
title: Value Object [DDD]
wordpress_id: 569
---

This metaphor does not fit completely, and the "Money-Example" is omnipresent, but I really like the pattern and so that's the story:

Let's say you have your grannies recipe for a chocolate cookie and you want to keep this in mind, but also you like to somehow experiment with some new or different ingredients. Normally you just keep in mind what you changed and later you write down the new recipe. But from time to time you might want to compare the two recipes or maybe you want to make cookies with the new and the changed recipes. In this situation it's really good to have both written down on paper. ;)

In the world of OOP you could think of a solution using the [Memento-Pattern](http://cookiepattern.blogspot.com/search/label/memento?max-results=100) but this doesn't really fit the situation and it's also some kind of overhead. That's the reason why the Value Object is the pattern of my choice.

So let's look at the recipes again - we want to add and remove ingredients without modifying the original recipe and we want to compare the resulting recipes. We don't need to track a special identity of our recipes since we're only "collecting" ingredients.

The idea of the Value Object is that every every method which somehow transforms the state of a object always returns a new object and the old object stays untouched. Whenever needed you should also implement a method to compare objects, I must concede that the common "Money Example" shows much better when that's needed ...

Another edge of this pattern in PHP is that you can use method-chaining to perform multiple actions within a single line of code. So just have a look at the example you will like it's taste :)


    
    
    class Recipe {
    	protected $ingredients;
    
    	public function __construct($ingred=”) {
    		$this->ingredients = implode(‘,’,array_unique(explode(‘,’,$ingred)));
    	}
    
    	public function addIncredient($ingred=”) {
    		return new Recipe($this->ingredients.‘,’.$ingred);
    	}
    
    	public function removeIncredien($ingred=”) {
    		return new Recipe(preg_replace(‘/,{0,1}’.$ingred.‘,{0,1}/’,”,$this->ingredients.‘,’));
    	}
    
    	public function printIncredients() {
    		echo str_replace(‘,’,‘<br></br>’,$this->ingredients);
    	}
    
    	public function equals(Recipe $recipe) {
    		return (strcmp($this->ingredients,$recipe->ingredients)===0)?true:false;
    	}
    }
    
    
    $granniesRecipe = new Recipe(‘flour,baking soda,sugar,salt,butter,vanilla,chocolate’);
    $aNewRecipe = $granniesRecipe->addIncredient(‘lemon’);
    $coconutRecipe = $granniesRecipe->removeIncredien(‘chocolate’)->addIncredient(‘coconut’);
    
    // check if that worked:
    echo ‘<br></br><strong> Grannies Cookie Recipe – Incredients are:</strong><br></br>’;
    $granniesRecipe->printIncredients();
    echo ‘<br></br><strong> A new Cookie Recipe could look like:</strong><br></br>’;
    $aNewRecipe->printIncredients();
    echo ‘<br></br><strong> A coconut cookie would look like:</strong><br></br>’;
    $coconutRecipe->printIncredients();
    


The implementation is a bit odd, since it only collects the names of the ingredients but not the amount, but including the amounts of incredients would not change the concept  and that's why I left it out. I hope you got a idea how the pattern works - the main thing is that there's no identity and that new objects are instantiated as soon as the state of the old one would change.
