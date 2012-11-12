---
layout: post
title: "Not using Gin or Guice"
description: ""
category: 
tags: [gin, guice, dependency, injection, DI, IOC]
---
{% include JB/setup %}
Up until  this point in my goal to have my engine support both GWT and Swing at the same time, I had been doing it by putting the gwt specific code 
in one repo, and the Awt/Swing stuff in another.

At this point, since I had both systems working well together, I decided to merge it back together. I didn't need to support both technologies at the same time, but it was more convenient to have both lots of code in the same repository, so they could be kept in sync.

I also had an ulterior motive: I wanted to support Gin or Guice.

I had taken a look at Gin when I was experimenting with Roo last year.
Roo generated GWT code that supported many best practices: JAspect, MVP, and Dependency injection via Gin. I had read that Gin (Gwt-INject) was the GWT version of Guice, which was a DI framework for Java, but never used it.

Once I started looking in to Gwt and Juice, I quickly found out that neither would work for code that compiles both in GWT and as POJ. Gin made heavy use of annotations, that worked in GWT, but would create errors in POJ. And vice verse for Guice.

So in the end, I went with plain plain old dependency injection. The standard pattern is to use class factories to create the different flavors of the objects. And have both flavors of that object implement the same interface. I also had an interface for the Factory, so that I could have a GWT implementation of the factory, and a Swing one. By doing this, I was also enable to better force that all the objects during one runtime could be either GWT or either AWT but never both. This allowed every GWT class to cast any Image it comes across to a GWTImage. Casting always feels like cheating, but I recognised this particular problem as one that Tim Sweeney (creator of unreal engine) had discussed in detail from way back in the 90s.. wel atleast the 2000's.... and casting is the only way around the problem.

