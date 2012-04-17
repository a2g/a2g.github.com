---
layout: post
title: "Farewell to Don Box's XML Property Bag (Part 2)"
description: ""
category: 
tags: [COM, unittest, TDD]
---
{% include JB/setup %}

Recapping from Part 1, a customer had discovered that several documents that were saved out containing our 3D technology had been corrupted. And the cause seemed to be an open source piece of code called the Xml Property Bag, that Don Box had written many years earlier....

Luckily, the critical mass of data was still in the document, but there needed to be some fixes applied to get these to be loaded properly. The way to fix it was to change some code hear where we integrated the XmlPropertyBag code. I stepped through it and found it a bit hard to follow. The code used many Xml DOM methods, but there were no strings in the debugger where you could see the state of the XML - so I added a fair bit of debugging code to monitor what was going on. It was about at this time I noticed two things:

-  XmlPropertyBag did far more than what we wanted, and in doing this extra stuff, it made tracing the code confusing.
-  What we wanted it to do was actually very simple.

So at that moment I decided to pull out XmlPropertyBag. I figured I could write it myself. Many times I'll get the impulse to rewrite something but I won't have the time, or the justification. But this time there were two things:

-  A severity zero bug, which meant I had the time.
-  A licensing burden with using the XmlPropertyBag code.

My debugging code had provided me with many examples of XML that was embedded in office documents, so I knew what I needed to be reading and writing. I took the test driven devlopment approach and made the tests first with this sample data. Then spent the rest of the day making the tests work. Making the tests work was harder than I had thought, but by the end of the day I had committed the new classes and the new tests. I quickly integrated these new classes with the code, and as of writing I have found no problems with it.

When we made the decision to switch to property bag (back in 2006) we used an opensource project when we fully didn't realise the implications. And this came back to sting us a bit. But I like to think that our impulse to go with Xml, an open standard, was the thing that saved us a bit, because it allowed us to easily rewrite the code without any reliance on using code that was unique, and difficult to write without copying.