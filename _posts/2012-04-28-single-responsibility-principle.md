---
layout: post
title: "Single Responsibility Principle"
description: ""
category: 
tags: [unittest, TDD]
---
{% include JB/setup %}

After I replaced [Don Box's XmlPropertyBag ](http://www.codeproject.com/Articles/3286/XML-Property-Bag-Implementation)., I printed out all the code it entailed, so as to show all the code that we no longer had to maintain, as a kind of prop, during the morning meeting. It was written as one behemoth class - pretty much representing the Xml document, whilst my code had the loading and saving code in two separate classes XmlIn and XmlOut.

This kind of conforms to the [Single Responsibility Principle](http://en.wikipedia.org/wiki/Single_responsibility_principle).

But that's not why I wrote it that way. I wrote it that way so it would be easier to test. The class only needed to do one thing at a time (either load from Xml or save to Xml). So the classes are basically one method to do the work, and a whole lot of others to retrieve the result.  

If you approached it from some non-test-centric angles you could also end up splitting the class in to smaller ones: The use cases don't ever require starting to load Xml, then save that xml, then keep loading - or anything like that, so you don't need to write code that allows that. 

But there are also many paths that would roll the functionality in to one: the overly object oriented case where someone just thinks of the resultant Xml document as an object, and all things related to it go in one class; or the lazy way where simply adding new files to version control is a bridge too far!

My general thinking on this at present is to keep things simple. State adds complexity, and adding combinations of state has a multiplicative effect with regard to complexity. If you can split that class in half and hide one lot of state from another, then you limit the combination of state, and limit the complexity. This is disregarding getters and setters of course, if you use those to expose your state then you kind of subvert that limitation.

Here is a great read regarding the causes of complexity, in a program:
(http://web.mac.com/ben_moseley/frp/paper-v1_01.pdf)


 