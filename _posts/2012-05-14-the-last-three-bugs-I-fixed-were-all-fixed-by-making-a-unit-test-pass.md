---
layout: post
title: "The last three bugs I fixed were all fixed by making a unit test pass"
description: ""
category: 
tags: [unittest, TDD]
---
{% include JB/setup %}

The last three bugs I fixed were all fixed by making a unit test work.
This is probably very poor in relation to some work places, but in our workplace it is a big deal.

One of the issues was a new feature of the script API, which I added a jsUnit test for (it's secretly an integration test, but we use the jsunit library).
The API features are the easy ones to test. If everyone was only writing libraries of api methods, then unit testing would be an easy job indeed!

But the other two weren't API features: it just so happened that their implementation had good unit test coverage, and so I was able to add these extra cases to the existing tests to verify them.

It may not seem like a big thing, but for just a fleeting few hours, everything seemed right. I hope I have more of this feeling :)