---
layout: post
title: "Start with your test data...."
description: ""
category: 
tags: [unittest, TDD]
---
{% include JB/setup %}

Am thinking about how to write a small class that traverses a directory structure, that it expects to find structured in a certain way, and then generates a bunch of output files, based on this structure.

In the past, I have just taken the route: Pass dependencies to the constructor, and make everything an interface. So that when I come to write unit tests for it, I can just write a mock implementation of one of those interfaces, and use that to test the class.

In the case of simluating a folder structure, you would have an IFolder or something that you can use to iterate through the files it has, and get access to any sub-IFolders it has.

And that would pass the reasonable test. And I'd code it first (non TDD) and wait for something to go wrong so writing test for it to be justified, and then write a test for it.

But if i'd have thought a little further (or even followed a TDD process) I would have discovered that I like test data for such a file/folder structure to be arranged in plain text. 

And this would mean that the test code that converts plain text to a dyanmic series of IFolders is quite a substantial, and probably needs testing itself. 

Which is bad. So what I probably should have done was make my test class take strings as input to begin with.

Hence "Start with your test data", when writing classes that you expect to be unit tested.... or do TDD, which will ensure this.

In hindsight, I probably shouldn't have such a thing for text driven test data. Constructing a hierarchy of data is not hard at all with a mock implementation of an IFolder.