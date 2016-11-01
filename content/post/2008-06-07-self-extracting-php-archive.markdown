---
author: tolleiv
categories:
- compression
- extracting
- php
- zip
comments: true
date: 2008-06-07T21:30:00Z
slug: self-extracting-php-archive
tags:
- compression
- extracting
- php
- zip
title: Self-extracting PHP archive
wordpress_id: 573
---

The __halt_compiler(); function in php enables to store some additional data in a php-file without blowing up the memory. A very nice possibility is to use this for a self-extracting php file as installation-packages of your php application.
A wile ago I created a script which automatically creates such a archive and I think you might like it....for the impatient ones: [Download](http://588299e40f6cb98516d7458.googlepages.com/createziparchive-0.5.1.php) / [Download as Zip](http://588299e40f6cb98516d7458.googlepages.com/createziparchive-0.5.1.zip).

Before I start to show how the entire script works I'd like to show you a small example so that you can see how the __halt_compiler(); function can be used to store some data at the end of a file:


    
    
    
    $fp = fopen(__FILE__, ‘r’);
    fseek($fp, __COMPILER_HALT_OFFSET__);
    $i=0;
    while($buffer = fgets($fp)) {
    	echo ($i++). “:”. $buffer.“<br></br>”;
    };
    __HALT_COMPILER();Line 1
    Line 2
    Line 3
    Last line 
    



The first thing you might mention is, that the closing "?>" is missing, but since the function-name is nearly self-explaining you should realize very fast what the output of the script might be ;)

If that's working the question is what's needed for a script which is meant to create and extract a archive? The first thing is a way to create the archive itself. I used the PHP-builtin [ZipArchive](http://de3.php.net/manual/en/ref.zip.php) for that. The second thing is a script which is able to extract this archiv (using the method show above). That's handled by the following snippet:

    
    
    
    try {
        $zipfilename = md5(time()).‘archive.zip’; //remove with tempname()
        $fp_tmp = fopen($zipfilename,‘w’);
        $fp_cur = fopen(__FILE__, ‘r’);
        fseek($fp_cur, __COMPILER_HALT_OFFSET__);
        $i=0;
        while($buffer = fread($fp_cur,10240)) {
            fwrite($fp_tmp,$buffer);
        }
        fclose($fp_cur);
        fclose($fp_tmp);
        $zipfile = new ZipArchive();
        if($zipfile->open($zipfilename)===true) { 
            if(!$zipfile->extractTo(‘.’)) throw new Exception(‘extraction failed…’);
        } else throw new Exception(‘reading archive failed’);
        $zipfile->close();
        unlink($zipfilename);
    } catch (Exception $e) {
        printf(“Error:<br></br>%s<br>%s>”,$e->getMessage(),$e->getTraceAsString());
    };
    __HALT_COMPILER();[zipdata is appended here later] 
    



As you see that's no rocket-science just read the data, pass it to the ZipArchive object via a temporary file and extract the archive.
If that's working then you need a script which brings the PHP extraction script and the zip-data together. And since we want to have a single script for the creation of our self extracting php archive, it would be very odd if we'd place the "template" for the extraction in a separate file. That's the reason why I just use the same method as above for this script and this time the data is php code instead of zip-data:


    
    
    
    try {
        $sourcefolder = ‘compressthis/’;                 // maybe you want to get this via CLI argument …
        $targetname = ‘phparchive.php’;                    
        $zipfilename = md5(time()).‘archive.zip’;         // replace with tempname()
        
        // create a archive from the submitted folder
        $zipfile = new ZipArchive();
        $zipfile->open($zipfilename,ZipArchive::CREATE);
        addFiles2Zip($zipfile,$sourcefolder,true);
        $zipfile->close();
        
        // compile the selfextracting php-archive
        $fp_dest =fopen($targetname,‘w’);    
        $fp_cur = fopen(__FILE__, ‘r’);
        fseek($fp_cur, __COMPILER_HALT_OFFSET__);
        $i=0;
        while($buffer = fgets($fp_cur)) {
            fwrite($fp_dest,$buffer);
        }
        fclose($fp_cur);
        $fp_zip = fopen($zipfilename,‘r’);
        while($buffer = fread($fp_zip,10240)) {
            fwrite($fp_dest,$buffer);
        }
        fclose($fp_zip);
        fclose($fp_dest);
        unlink($zipfilename);
        
    } catch (Exception $e) {
     echo $e->getTraceAsString();
    }
    
    function 
    addFiles2Zip(ZipArchive $zip,$path,$removeFirstFolder=false) {
        $d = opendir($path);
        while($file = readdir($d)) {
            if ($file == “.” || $file == “..”) continue;
            $curfile=($removeFirstFolder)?substr($path.$file,strpos($path,‘/’)+1):$path.$file;
            if(is_dir($path.$file)) {
                $zip->addEmptyDir($curfile);
                addFiles2Zip($zip,$path.$file.‘/’,$removeFirstFolder);    
            } else {
                $zip->addFile($path.$file,$curfile);
            }    
        }
        closedir($d);
    }
    
    
    __HALT_COMPILER();[the script shown above]
    



If you all wrap up into a single script the you'll have something like the [script I already mentioned.](http://588299e40f6cb98516d7458.googlepages.com/createziparchive-0.5.1.php)
I think that it could be useful to have a version with a better error-handling and maybe also some CLI functions so that there's no need to edit the script itself everytime... I'll keep you updated as soon as I have something like that :)
