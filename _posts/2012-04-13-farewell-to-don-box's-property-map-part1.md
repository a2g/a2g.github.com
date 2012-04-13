---
layout: post
title: "Farewell to Don Box's XML Property Bag (Part 1)"
description: ""
category: 
tags: [COM, unittest, TDD]
---
{% include JB/setup %}

A long, long time ago, I instructed one of my team mates to use [Don Box's XmlPropertyBag implementation](http://www.codeproject.com/Articles/3286/XML-Property-Bag-Implementation). The case was that we were maintaining an ActiveX control, and it had many properties in the [PROPERTY_MAP](http://msdn.microsoft.com/en-us/library/y5s333c2(v=vs.71).aspx) that we wanted to remove. It looked like someone had wanted a very large amount of state to be persisted to the Office document when you saved the control - which is fair - but the way they did it was to add many property methods, and put these properties in the property map.
This was a problem when inserted in to various html designers that took a snap shot of all the properties in the property map, and embedded it in to the html.
(The original developer should have probably used an extra stream, not the properties stream). 

Anyhow we needed to remove these properties from the property map, but the standard way ATL persisted properties in to a stream was just to write them one at a time, in order, based on their binary size. So if we changed the property map - even a little bit - it would break all loading of properties. The PROPERTY_MAP really should have been called a PROPERTY_LIST.

So we decided, instead of writing multiple properties to the stream, why not write a single XML string to the stream, in which all the properties can be stored and retrieved as with random access. That way we don't ever get locked in to a particular PROPERTY_MAP structure again, and we could handle backwards compatibility by leaving in one legacy stream reader that read the properties in the order they were.

I looked around at the time, and Don Box, in his wisdom, was already on to that problem and he had made an XmlPropertyBag class. And another guy called Jörgen Sigvardsson had taken this code, improved it, and put on Code Project. It looked like the exact thing we needed so we changed our ActiveX code to use it.
