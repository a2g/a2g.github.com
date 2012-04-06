---
layout: post
title: "Avoiding bad logic in unit tests"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I was testing a method for support for UTF8 conversion the other day, and at first I had the japanese text
written as string literals in the code (by setting the file to use a particular codepage) - and then I had a method
that converted the japanese text to UTF-8, for use as input for a method.

But this is bad, because my code to convert code page to UTF8 might fail, and so the only proper way is
to test literal input, with literal output. Roy osherove talks about this in his blog:
http://weblogs.asp.net/rosherove/archive/2010/08/14/two-different-ways-to-create-bad-logic-in-unit-tests.aspx


In this case, literal UTF-8 input is an array of unsigned chars

    // arrange
    unsigned char japaneseKanjiEncodedInUtf8[] = {0xe9, 0x80, 0xb1, 0xe5, 0xa0, 0xb1};

    CComSafeArray<byte> kanjiAsUtf8;
    kanjiAsUtf8.Add(_countof(japaneseKanjiEncodedInUtf8), japaneseKanjiEncodedInUtf8); 
    CComVariant kanjiAsUtf8Var(kanjiAsUtf8);

    // act
    wchar_t kanji0 = XString(kanjiAsUtf8Var).operator BSTR()[0];
    wchar_t kanji1 = XString(kanjiAsUtf8Var).operator BSTR()[1];
  
    // assert
    BOOST_REQUIRE(kanjiAsUtf8Var.vt == (VT_ARRAY|VT_UI1));
    BOOST_REQUIRE(kanji0== 36913);
    BOOST_REQUIRE(kanji1== 22577);