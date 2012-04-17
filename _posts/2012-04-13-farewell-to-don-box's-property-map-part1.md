---
layout: post
title: "Farewell to Don Box's XML Property Bag (Part 1)"
description: ""
category: 
tags: [COM, unittest, TDD]
---
{% include JB/setup %}

A long, long time ago, I instructed one of my team mates to use [Don Box's XmlPropertyBag implementation](http://www.codeproject.com/Articles/3286/XML-Property-Bag-Implementation). The case was that we were maintaining an ActiveX control, and it had many properties in the [PROPERTY_MAP](http://msdn.microsoft.com/en-us/library/y5s333c2.aspx) that we wanted to remove. It looked like someone had wanted a very large amount of state to be persisted to the Office document when you saved the control - which is fair - but the way they did it was to add many property methods, and put these properties in the property map.
This was a problem when inserted in to various html designers that took a snap shot of all the properties in the property map, and embedded it in to the html.
(The original developer should have probably used an extra stream, not the properties stream). 

Anyhow we needed to remove these properties from the property map, but the standard way ATL persisted properties in to a stream was just to write them one at a time, in order, based on their binary size. So if we changed the property map - even a little bit - it would break all loading of properties. The PROPERTY_MAP really should have been called a PROPERTY_LIST.

So we decided, instead of writing multiple properties to the stream, why not write a single XML string to the stream, in which all the properties can be stored and retrieved as with random access. That way we don't ever get locked in to a particular PROPERTY_MAP structure again, and we could handle backwards compatibility by leaving in one legacy stream reader that read the properties in the order they were.

I looked around at the time, and Don Box, in his wisdom, was already on to that problem and he had made an XmlPropertyBag class. And another guy called Jorgen Sigvardsson had taken this code, improved it, and put on Code Project. It looked like the exact thing we needed so we changed our ActiveX code to use it.

Six years passed, and the XmlPropertyBag code worked admirably. There had only been a few bugs found in it soon after we put it in to production. But after that, they all went away. Until now.

About six months ago we heard from a customer that had embedded much 3D content in to Word documents using our ActiveX control, but when they upgraded to our latest version, they couldn't see it anymore.

Around about the same time, our whole company's code was [scanned by BlackDuck](http://en.wikipedia.org/wiki/Black_Duck_Software) to determine which open source licenses we were using: to make sure we were legal. The XmlPropertyBag code posed a problem because although [Jörgen Sigvardsson's code](http://www.codeproject.com/Articles/3286/XML-Property-Bag-Implementation) came with an acceptable license, his code was in some way based on Don Box's code when he [was working at Developmentor](http://www.sellsbrothers.com/links/dbox/xmlpropbag.zip). And this part was a cause of concern in our legal department, we didn't really have persmission from DonBox to use this code.

The legal problem moved slowly, but the customer problem was quickly raised in to a top priority error, and I was made to investigate it. The problem seemed to be caused by some old documents that had not saved out the full list of properties. Our lead QA person tried exhaustively to reproduce it - with the full suite of previously released products at her disposal - but had been unable to reproduce it. My guess is that somehow one of the properties threw an exception, or returned non zero, when it was being read, and ATL stopped writing the list midway. But regardless of that, we had documents now that were failing to load and I needed to fix it.