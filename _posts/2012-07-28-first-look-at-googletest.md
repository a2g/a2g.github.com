---
layout: post
title: "First look at googletest...."
description: ""
category: 
tags: [unittest, TDD]
---
{% include JB/setup %}

GTest has been around for ages, but so we've been using boost test at work, and it's fine.

But, I had a home project that needed testing, and I thought that adding a depedency on the sprawling boost framework might be a bit too much.

And after going and downloading it, this turns out to be true: Boost is a 50MB download, GTest is less than 2MB ( I used version 1.6)

Probably the biggest benefit of gTest is forms the basis of "gmock" - a mocking framework. Boost framework wasn't made to support mocking. 

Here are some other things I noticed:

- gmock is the first killer feature that boost doesn't have.

- the second feature that boost doesn't have is "more types of tests". Boost only has a few testing macros, BOOST_REQUIRE, and BOOST_CHECK and some others. All take a boolean parameter, which is where you put your test expression. googletest allows you to be more specific and say that you are checking for equality, and then allows you to enter two parameters: expected, and occured. This gives you a benefit at failure time, where the test can give more information: not only that it failed, but what it got, and what was expected. This can give you an idea of just what might have triggered the error, and you might figure out a solution whilst reading the build report.

- Its multiplatform. To support multiplatforms, google ships it with MAKE files. And for the spoiled Visual Studio users who can't handle MAK files, Google ships it with Visual Studio 2001 vcprojs, and relies on new VC's ablity to open and upgrade any old vcproj to suit a newer Visual Studio. 

- it's cpp files are given the .cc extension

- you only need to include one .cc file to make your test link: C:\gtest-1.6.0\src\gtest-all.cc\. Whereas boost, you need to hunt around for the 6 .cpp files to include.

- It allows for fixtures, whereby a bunch of test methods can share a buildup() and a teardown() method. This isn't such a big bonus, since you can simply call such methods manually, or build an RAII class. In Boost, if many checks/requires need to share the same set up, then most of the time I'll put that in to the same test. Putting more checks in a single test helps prevent cases where you fix one test and break another (ala Whack-a-mole).

- as is the standard, googletest outputs to stdout. So any build process can be made to scan it's output.

