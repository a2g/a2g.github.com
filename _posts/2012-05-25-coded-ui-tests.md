---
layout: post
title: "coded-ui-tests"
description: ""
category: 
tags: [mstest,coded,codedui,codeduitests]
---
{% include JB/setup %}

We maintain an activeX control.

ActiveX controls have a programmatic interface that alends itself to automation of testing. 
But it has a big user interface - so it needs automated functional testing. 

Initially, we chose TestComplete for this, because it was very powerful.  Recently we became aware that Visual Studio Premium had a thing called Coded-UI-Tests that did much the same thing - but the MS solution didn't support Qt, and since we had much of our code in Qt, we decided to stay with Test Complete.

But since we could only afford a few copies, it meant that none of the developers could construct automated ui tests for things, as we were coding something up. So for one of our recent sprints, I had a go at building a little demo project for testing the ActiveX control in a web page.

The first challenge was to have the code findthe web page. In release mode, it runs with a different 'test directory' folder to the one it uses in debug mode. So I added a piece of code that tries to load using the debug relative path, and then tries to load using the release relative path, and uses the first drectory that works.

Next challenge is image comparison. Coded UI tests have a great facility for capturing images (CaptureImage), but they don't have any technology to compare images. 


At first I used Bitmap.GetPixel but this was very slow.
http://stackoverflow.com/questions/3433996/fast-comparison-of-two-bitmap-objects-on-a-pixel-per-pixel-basis

So I went with compare:
http://blogs.msdn.com/b/domgreen/archive/2009/09/06/comparing-two-images-in-c.aspx

This worked great for the Sprint Meeting. For the next step, I would like to get the html pages running in chrome or firefox. Visual Studio supports Internet Explorer very well, but you are on you're own for the other browsers.

