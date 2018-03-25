---
layout: post
title: "Smells like bad inheritance"
date: "2018-03-24 19:55:12 -0700"
---

About a month ago, I had the opportunity of refactoring a part of our codebase at work. Specifically, I had my focus on a cluster of classes linked together by what seemed to be a vague hierarchy. The classes had to do with creating HTTP Request objects for various calls made to different systems. Of late, we had had multiple complaints of severe performance degradations somewhere in that clump of classes. We addressed complaints on an ad-hoc basis for a while. But multiple patches later, it was finally decided that we needed to deal with the root of the problem before things got out of hand.

The original authors of the classes have long left our organization, so it was left to me and my handy tools (Git and our SDLC management system) to try and figure out how the classes were intended to work and why they had been structured and coded the way they were. A few days of digging around later, it was fairly obvious that the abstract classes of the hierarchy had been severely abused and used as a dumping ground for code that needed to be used by one or more child classes. As a result, each child class initialization got bloated, whether it needed all that code or not.


![](../images/2018-03-24-smells-like-bad-inheritance-1.png)


So began the long painful exercise of de-coupling classes and the code that they relied on, into sensible objects that would be accessed through composition if a has-a relationship made sense or we could simply create and destroy instances of 'helper' classes as and when desired. Either way, it would greatly reduce the overhead of initialization for each of the child classes.

It is unfortunate that Java allows inheritance as a pattern for code-reuse. For small projects it might work great, but when it comes to enterprise-ready and scalable software, it is best to avoid such a practice as it can quickly get out of hand and result in a [God Object](https://en.wikipedia.org/wiki/God_object). Some programmers like to refer to such a practice as 'code smell'. It is just a way of saying 'anti-pattern', without sounding authoritarian. Use of inheritance as a code-sharing mechanism more often than not violates the [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle).

When it comes to quality, scalable Java code, I strongly feel, inheritance must only be used as a way to implement polymorphism. I have seen discussions online where people have come to the conclusion that inheritance itself is bad. But, I do not agree. It is a powerful feature of OOO design that can be easily misused in Java.

PS: In case you are wondering how I drew up the beautiful class diagram : [UMLetino](http://umletino.com/)
