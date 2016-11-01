---
author: tolleiv
comments: true
date: 2010-01-23T17:19:42Z
slug: debugging-mod_rewrite
tags:
- apache
- debug
title: Debugging mod_rewrite... my favorite way
wordpress_id: 299
---

Once a month someone is asking because he has issues to get his mod_rewrite rules to do what he want's. Writing the rules and the required RegEx for these rules is quite easy, but Apache still behaves strange every now and then and that's where one of my favorite ways to "debug" mod_rewrite comes in very handy. And I felt that writing something is better than having a silent blog ;)

The following block is what it's all about. It seems to have it's origin on [WebmasterWorld](http://www.webmasterworld.com/forum92/5360.htm) and [Rob Russel's Blog](http://www.latenightpc.com/blog/archives/2007/09/05/a-couple-ways-to-debug-mod_rewrite) and looks basically like this:


    
    RewriteCond %{QUERY_STRING} !vardump
    RewriteRule (.*) http://www.example.com/$1?vardump&reqhost=%{HTTP_HOST} [R=301,L,QSA]



Pretty easy and quite nice. The first line just prevents recursion, so that you'll be able to see the first redirect and nothing else. The second line is where you can place in every information you'd like to check. The [R=301,L,QSA] makes sure that you can debug without interruption. The "R=301" makes sure the browser doesn't start a second request, but tells him that the location was changed, the "QSA" makes sure that the querystring isn't lost and the "L" prevents that your server performs other redirection rules.

As shown the first block already unveils some information and places it into the redirection URL, so you could use it to check what the value of HTTP_HOST is and you also know whether mod_rewrite works or not. This way you can debug every variable mod_rewrite offers and you can check environmental variables and results of the regular expressions.

If you'd like to know whether a specific RewriteCond is working or not just place it in that block and you'll see if it's still redirecting or not, like this ([via](http://twitcode.org/Bb)):


    
    RewriteCond %{QUERY_STRING} !vardump
    RewriteCond %{HTTP:Accept-Language} ^de.*$
    RewriteRule (.*) http://www.example.com/$1?vardump&languageMatchedDE [R=301,L,QSA]



Checking a regular expression from an RewriteCond looks like this (used on a server with a pretty strange setup for %{DOCUMENT_ROOT}):


    
    RewriteCond %{QUERY_STRING} !vardump
    RewriteCond %{REQUEST_FILENAME} ^(.*\/htdocs\/).*$
    RewriteRule (.*) http://www.example.com/$1?vardump&reqfilename=%1 [R=301,L,QSA]



As you see that's a pretty handy way to debug lot's of mod_rewrite stuff, it helped me quite often and I hope it does the same for you :)
