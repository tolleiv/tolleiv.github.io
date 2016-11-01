---
author: tolleiv
categories:
- offtopic
- shellscript
comments: true
date: 2008-07-10T21:55:00Z
slug: track-time-for-a-subroutine-within-a-shell-script
tags:
- bash
title: track time for a subroutine within a shell-script
wordpress_id: 587
---

The following code-snippet is a shell-script which does the following:




	
  1. Track the time for a block of shell commands

	
  2. Check if the time was less than x seconds (the example uses 10 seconds)


	
  3. If the block run through too fast the script waits/sleeps a few seconds

	
  4. Run all this within a loop so that the block of shell-commands is executed periodically


I used the script combined with a PHP script which processes a queue. The PHP script processes 1000 elements from the queue and takes about 30 seconds for that. Since just having a cronjob per minute would be not efficient enought I used this script.
The waiting-block is necessary because now and then the queue is empty ... but I think there are lot's of situations where a script like this can be usefull:



    
    
    #!/bin/bash
    while [ 1 -ge 0 ]; do
    time_begin=`date +%s`
    
    
    ###BLOCK 2 TRACK - BEGIN
    number=$RANDOM
    let "number %= 20"
    sleep $number
    ###BLOCK 2 TRACK - END
    
    time_end=`date +%s`
    total=$((time_end-time_begin))
    
    if [[ $total -ge 10 ]]; then
    echo "time taken was: $number : $total"
    else
    echo "time take was too less  $number : $total"
    sleep 10
    fi
    done
    
