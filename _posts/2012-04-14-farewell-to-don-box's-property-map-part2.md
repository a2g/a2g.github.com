---
layout: post
title: "Farewell to Don Box's XML Property Bag (Part 2)"
description: ""
category: 
tags: [COM, unittest, TDD]

# Farewell to Don Box's XML Property Bag (Part 2) #
Six years past, and the XmlPropertyBag code worked admirably. There had only been a few bugs found in it soon after we put it in to production. But after that, they all went away. Until now.

About six months ago we heard from a customer that had embedded much 3D content in to Word documents using our ActiveX control, but when they upgraded to our latest version, they couldn't see it anymore.

Around about the same time, our whole companies code was [scanned by BlackDuck](http://en.wikipedia.org/wiki/Black_Duck_Software) to determine which open source licenses we were using: to make sure we were legal. The XmlPropertyBag code posed a problem because although [Jörgen Sigvardsson's code](http://www.codeproject.com/Articles/3286/XML-Property-Bag-Implementation) came with an acceptable license, his code was in some way based on Don Box's code when he [was working at Developmentor](http://www.sellsbrothers.com/links/dbox/xmlpropbag.zip). And this part was a cause of concern in our legal department, we didn't really have persmission from DonBox to use this code.

The legal problem moved slowly, but the customer problem was quickly raised in to a top priority error, and I was made to investigate it. The problem seemed to be caused by some old documents that had not saved out the full list of properties. Our lead QA person tried exhaustively to reproduce it - with the full suite of previously released products at her disposal - but had been unable to reproduce it. My guess is that somehow one of the properties threw an exception, or returned non zero, when it was being read, and ATL stopped writing the list midway. But regardless of that, we had documents now that were failing to load and I needed to fix it.

Luckily, the critical mass of data was still in the document, but there needed to be some fixes applied to get these to be loaded properly. The way to fix it was to change some code hear where we integrated the XmlPropertyBag code. I stepped through it and found it a bit hard to follow. The code used many Xml DOM methods, but there were no strings in the debugger where you could see the state of the XML - so I added a fair bit of debugging code to monitor what was going on. It was about at this time I noticed two things:

-  XmlPropertyBag did far more than what we wanted, and in doing this extra stuff, it made tracing the code confusing.
-  What we wanted it to do was actually very simple.

So at that moment I decided to pull out XmlPropertyBag. I figured I could write it myself. Many times I'll get the impulse to rewrite something but I won't have the time, or the justification. But this time there were two things:

-  A severity zero bug, which meant I had the time.
-  A licensing burden with using the XmlPropertyBag code.

My debugging code had provided me with many examples of XML that was embedded in office documents, so I knew what I needed to be reading and writing. I took the test driven devlopment approach and made the tests first with this sample data. Then spent the rest of the day making the tests work. Making the tests work was harder than I had thought, but by the end of the day I had committed the new classes and the new tests. I quickly integrated these new classes with the code, and as of writing I have found no problems with it.

When we made the decision to switch to property bag (back in 2006) we used an opensource project when we fully didn't realise the implications. And this came back to sting us a bit. But I like to think that our impulse to go with Xml, an open standard, was the thing that saved us a bit, because it allowed us to easily rewrite the code without any reliance on using code that was unique, and difficult to write without copying.