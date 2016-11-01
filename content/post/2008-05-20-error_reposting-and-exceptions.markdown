---
author: tolleiv
categories:
- errors
- exceptions
- php
comments: true
date: 2008-05-20T21:40:00Z
slug: error_reposting-and-exceptions
title: error_reposting and exceptions....
wordpress_id: 564
---

I just found this line:  


  
set_error_handler(create_function('$x, $y', 'throw new Exception($y, $x);'), E_ALL~E_NOTICE);  


  
in the comments on php.net and did a short test to see whether it works. Normally I used to have a global function which passes all the errors through a logging-chain (FILE/MAIL/DISPLAY) but having exceptions could be more comfortable because you might sometimes want to make decisions locally and not on a global scope and just setting and re-setting the error-handler is not the best idea for this... I'm not sure if I get used to it, because the error-handling function did a good job for the last years ... we'll see :)  
So that's the full example code:  


  
set_error_handler(create_function('$x, $y', 'throw new Exception($y, $x);'), E_ALL);  
try {  
  
$errors = array(  
                    E_USER_WARNING => "something might be wrong",  
                    E_USER_NOTICE => "it works but I don't like it",  
                    E_USER_ERROR => "something went completly wrong",  
);  
  
//    echo $name; //should cause a E_NOTICE  
  
foreach($errors as $code=>$msg) {  
    try {  
        trigger_error($msg,$code);  
    }  
    catch (Exception $e) {  
        if(!in_array($e->getCode(),array(E_USER_WARNING,E_USER_NOTICE))) {  
            throw $e;  
        }  
        else {  
            // not a big deal we can push this into a log-file and continue or work...  
            echo "local exception: (".$e->getCode(). ") ".$e->getMessage()."<br/>";  
        }   
    }  
}  
  
}  
catch (Exception $e) {  
echo "global exception:".$e->getMessage()."<br/>";  
}  
  


  

