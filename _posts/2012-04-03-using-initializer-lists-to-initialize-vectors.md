---
layout: post
title: "Using initializer lists to initialize vectors"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I was writing code in a unit test that initialized a vector with values. And putting each push_back() on a separate line. Wishing that we were able to use the C++11 extensions at work, and initialize the vector on one line (similar to how you can statically initialize an array).

That's when I thought about using to do it, and sure enough its quite easy. 

So to write code like this:

	const char* names[] = {"n0","n1","n2","n3","n4","n5","n6","n7","n8","n9","n10","n11""};
	std::vector<std::string> namesAsVector = StringsToStringVector(names);

...all you need is this:

	template<class T> std::vector<std::string> StringsToStringVector(T& arrayOfStrings)
	{
    	std::vector<std::string> vectorOfStrings;

    	int count = (sizeof(T)==0)? 0 : sizeof(T)/sizeof(arrayOfStrings[0]);
    	for(int i=0;i<count;i++)
    	{
        	vectorOfStrings.push_back(arrayOfStrings[i]);
    	}

    	return vectorOfStrings;
	}


This sort of thing is very useful in unit tests where you need to set up lots of test data, to test in your tests.
