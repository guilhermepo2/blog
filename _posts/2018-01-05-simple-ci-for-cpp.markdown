---
layout: post
title: Setting Up Circle CI on GitHub for C++ and CMake
date: 2018-01-10 10:37:20 -0200
description: Ever wanted to set up your own continuous integration? You can do that with GitHub and CircleCI. Check out how I did for C++ and CMake!
img: mistake.jpg
tags: [C++, CI, Programming]
---

It’s been 3 months or more since I started my internship on automated testing. I studied and learned about docker, testing libraries, automated testing (of course), continuous development and, finally, continuous integration. Knowing about all of this made me a programmer twice as better than I was. This is a lot of improvement in 3 months and some days. So I started to pick interest in testing related tasks also on my personal projects and on my most recent one I decided to set up a CI environment on GitHub.

I had a bit of trouble configuring it at first and I couldn't find a lot of help from the internet, I had to pick some things here and there but there wasn't an unique resource about what I wanted to do. Well, now there is since I decided to write about it.

## What is Continuous Integration ? (CI)

**Short Version:** It is building a pipeline that will be executed in every commit at your master branch on git, this pipeline builds (usually using a container) the whole environment needed for the tests and execute all the tests you've coded. If all tests pass, you are happy and the changes were successful. If any test fail, you can fix it as soon as possible, this is better than finding out about bugs when you wrote 1000 lines of extra code relying on that buggy function.

**Long Version:** It is basically the same as the short version, but with a lot of Management stuff together. Let's do it.

When Software Engineering, Computer Programming and these things were new, the usual thing to do was to interview a client, gather all they would need in the project and develop it without much contact with each other, everything was well documented, signed and agreed by both parts, what could possibly go wrong?

Except that everything goes wrong. If you have ever worked with a client in your job or as a freelancer, you know that a lot of things changes and if the final product is 50% of the intended on the beggining of the project you are lucky.

The way to go nowadays is to use Agile methodoligies (i.e. Scrum) and deliver to the client as fast as possible, there is still the need to interview them and write down everything will be needed. But instead of developing away from the client, the system is divided in small parts, as much as possible, and each part is delivered when complete, the idea is to deliver something small every week, every 2 weeks, every month, etc...

That's where Continuous Integration kicks in.

As deliveries are fast, also are development, commits and everything else basically. So you **need** a way to verify that every new feature won't change what is already there. Imagine your client happy with two pieces of your product, you give to them the third piece but it breaks the other two, now you have angry clients.

If you have good software testing, good automated testing that covers a big part of what is ready in the product, it is possible to verify if everything is working as it should be.

Now it would be even better to automate the automated testing and do it once in a while or when something changes! Now you go back and read the short version again. 

![No More Rubber Duck Debugging!]({{site.baseurl}}/assets/img/debugging-finn.jpg)

*No More Rubber Duck Debugging! Or... Plastic Finn Debugging, anyway... You got it.*

## What we are going to do?

## Doing it.

