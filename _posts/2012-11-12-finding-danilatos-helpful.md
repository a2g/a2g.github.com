---
layout: post
title: "Finding Danilatos helpful"
description: ""
category: 
tags: [unittest, TDD]
---
{% include JB/setup %}

I'd watched the IO videos long ago: Ray Ryan, two years in a row,
heralding the benefits of Google's MVP. I'd used Microsofts MVC and saw
that MVP was simpler, so I used MVP. I'd previously just been using a
two-class system loosely based on what Qt forced you to do, and so 
I switched to a three class based system with a simple class as the model
(what Java would call a POJO) and a View class that was the only
class in the trio exposed to GUI classes (and UIBinder as such).

But I still really didn't *get* MVC.

MVC's main advertised benefit comes from testing.

After listening to Danilatos talk carefully


I'd just written a test of my CommandLinePresenter
which has a very simple view (a single Label)
and a very simple model (a few strings that make up that label)
and now it was time to test the InventoryPresenter.

For my CommandLinePresenter, my testing code looked something like:
mouseMove(x,y)
mouseClick(x,y)
at which point, the test would recieve a call on a methd of an interface
it had implemented, and it asserted this data in it's test.

For the next class, the InventoryPresenter, I was hoping to do the same.
But then I started thinking that maybe I should put similar methods on
my View, because it's a gui class. Then I thoguht maybe I should change
the way I'm testing these classes? But then the MVC dogma kicked in: and 
I was left with no doubt that the best way to test was to make the
view "correct by construction" - it's not being tested, so don't put anything
in there: it's got to go in to the InventoryPresenter.

I realised that this has larger implications on the dependency injected
views I'd implemented last month. When I did that piece of refactoring,
I'd added some little differences in the way the views are implemented on
each system. But I realised at this point, that, if I am to share the same
Presenter code for each system, then the views on each system
are going to have to be pretty much identical. And if they need to differ 
in a large way, then not only would I have methods to create the views 
in my Factory interface, I would also need methods to create presenters.
Hence the need for Dependency Injection frameworks, where the
entire tree of classes is specified through annotations and such.
