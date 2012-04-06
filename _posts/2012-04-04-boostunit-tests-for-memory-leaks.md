---
layout: post
title: "Boost::unit tests for memory leaks"
description: ""
category: 
tags: [leaks,Qt,unittest]
---
{% include JB/setup %}


I've just noticed that the Boost unit test framework checks for memory leaks. 

Whilst boost is running the c++ tests in a particular project, it also tests for memory leaks. So if one of your tests passes, but has a memory  leak, then instead of continuing on to the next test, boost will print a memory leak dump to output, and not run anymore tests.

I found this out whilst switching a unit test that constructs an Xml document from using  our own internal Xml writing class, to using Qt's QXmlDocument. The memory leak comes from using the .toString() method of the class. It's a pretty bad bug. Even constructing an empty QXmlDocument, and calling toString(). And Qt [don't look like they've fixed it yet](https://github.com/mojombo/jekyll/wiki/Sites). 

I guess Qt doesn't use boost to do their unit testing!


