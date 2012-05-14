---
layout: post
title: "Dependency free classes are worth it"
description: ""
category: 
tags: [unittest, TDD]
---
{% include JB/setup %}

Last month I got to work on feature, which allowed me to write some fresh code. I knew the core mechanism of the feature could be done using only native types, and container classes, so I kept *the class* free of dependencies so I didn't need to write mock objects or anything like that. 

Fast forward to last week, and the feature had a few additional points were added to the feature specification. The existing mechanism requirend lots of manipulation of nodes from our object model.  I thought I may need to rewrite *the class*, or atleast add dependencies to it, that would require a mock object to test. But when I sat down and looked at the code, I found a way to do it by keeping *the class* exactly the same, and just wrote another class, with dependencies that manipulated it. 

In the end, I was quite happy with how I avoided adding dependencies to that core class, because keeping core mechanisms free of dependency makes them easier for others to understand, and easier to maintain. I wonder if I was given the revised spec at the beginning, whether I would have distilled to core mechanism in to its own class, free from dependencies on other classes. Probably not. I will probably try an extra bit harder to see if there is a way to split dependency free classes off of classes in the future. 