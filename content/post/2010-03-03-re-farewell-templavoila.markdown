---
author: tolleiv
comments: true
date: 2010-03-03T23:04:08Z
slug: re-farewell-templavoila
tags:
- templavoila
- typo3
title: 'Re: Farewell, TemplaVoila!'
wordpress_id: 356
---

Dmitry decided to leave the TemplaVoilà project today and he handed the extension-leadership to me. Since this is a very abrupt change and since there was lot's of (mis)communication involved I'd like to use this blogpost to answer his ["Farewell, TemplaVoila!"](http://dmitry-dulepov.com/article/farewell-templavoila.html) post (he turned off the commenting function).

As you might know Steffen Kamper and I joined the TemplaVoilà team some time ago and since Dmitry wasn't very motivated to maintain the extension anymore, we took over and tried to make TemplaVoilà ready for TYPO3 4.3 _(see Dmitry's clarification below)_. We also tried to get rid of the over 250 listed bugs from bugs.typo3.org. This went fine for quite some time and we released version 1.4.0 and 1.4.1 in November parallel to TYPO3 4.3. Unfortunately the release come a little too fast for us and a couple of major bugs couldn't be fixed by that time. After that release we had a meeting with Dmitry and we all agreed to release 1.4.2 in the beginning of January. We found and fixed tons of bugs and also implemented long awaited features in the meantime. We also had the luck that others found new motivation and started to send feedback and started to test TemplaVoilà with us. (Special thanks to Uschi, Jeff and Ron).
Three weeks ago we decided that the current state is "ready to release" and we told Dmitry that it's up to him to release 1.4.2. Quite some time passed by and today he used several channels (Twitter, Facebook and Newsgroups) to tell the world what he found:



> _No TV 1.4.2 release soon. Found a bug in page module with unlinking in 1 minute after starting tests. I am severely disappointed by this :(_



He was right - a very obvious bug showed up in the page-module within TYPO3 4.2. Unfortunately nobody saw it and it also seems that some of us are still unable to reproduce it. Anyways --- in my opinion --- he choose the wrong way to communicate that. Instead of talking to Steffen and me, he decided to talk to anyone else. I wrote him a mail and told him that I didn't like the way he brought this up and asked for a Skype meeting to discuss how to proceed. After serveral emails back and forth and after others joined the communication, Dmitry decided (without anyone of us asking for it) to leave the team and hand over the leadership. To make it clear to everyone: the discussion started because of one JavaScript error and a few icons in the backend which he didn't like very much. The Skype meeting never happend - although it would have saved lot's of time and confusion for all of us.

I'm not very happy with his decision, because this leaves lot's of questions and because we loose a very diligent developer. But it seems that there's currently no way to convince him that he might be wrong. 

As team Steffen and I will try to continue the development and improvement of TemplaVoilà, especially because it's one of the most important benefits TYPO3 can offer. I ask everyone to join and contribute some time for testing or feedback. I dislike Dmitry's approach to fork TemplaVoila and host it somewhere else _(see Dmitry's clarification below)_. I'm inviting him to rejoin the team at any time and work in a constructive way with us. I'm still convinced that all this just happend because miscommunication and not because of "real" issues.

---
In addition it also seems that others offended Dmitry and asked him _not to stop the process anymore_ - I understand his distraction. He did a very good job in the past, he was open for our improvements and his biggest concern was quality not power or money or anything. Being offended for something you do in your freetime without any payment and being offended by people, who earn money from their clients with your (free) work, is always distracting. Please think about that in future communications with any open source software developer.

---
If you wonder how to get in touch with us: http://bugs.typo3.org lists all known bugs and their status. Report new bugs there or try to add new information to existing ones. Team discussion, in regards to fixed bugs or new features, happens, compareable to the [TYPO3 Core](http://typo3.org/teams/core/core-mailinglist-rules/), in the "typo3.team.templavoila" list on "lists.typo3.org". If you need regular feedback or help with TemplaVoilà please use the "typo3.projects.templavoila" list.

---
* Update 05.03.2010: After I posted that the link to that blogpost on Facebook, Dmitry commented it and just to be fair, that's what he said:


> _Thanks for your post. It is fair and explains the situation well. I would only clarify two moments: (1) I am not forking TV but creating a version for myself. I do not plan to release it to TER or anywhere. It is a TV for me as personally I like it (2) "Dmitry wasn’t very motivated to maintain the extension anymore" is not exactly so, rather to my taste TV already did all it had to do. I invited others to move TV further because I did not need anything else from it. Thanks again._




