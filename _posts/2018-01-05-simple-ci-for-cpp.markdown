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

## What are we going to do?

Basically setting up a really simple project with C++ and CMake, write tests for it and then use Circle CI to test it at every update.

1. **The C++ Code:** I will use a simple class I'm using in a current project, the *Point* class, it has two attributes, x and y, and some methods to manipulate them, that's all.
2. CMake File.
3. Writing Tests for the Point Function.
4. Building and Running Locally the Tests.
5. Setting Up Circle CI.

## Doing it.

This is by no means a C++, CMake, Unit Testing or Catch sort of tutorial, I'm assuming that you know them or have at least some familiarity to understand the code below. Anyway, the code should be pretty straightforward for anyone with some programming background.  The code is publicly available [here](https://github.com/guilhermepo2/cpp-cmake-circle-ci)

### The C++ Code
#### point.hpp
```c++
#ifndef __POINT__
#define __POINT__

class Point {
private:
    int x;
    int y;
public:
    Point() { this->x = 0; this->y=0; }
    const inline int getX() { return this->x; }
    const inline int getY() { return this->y; }
    inline void setX(int x) { this->x = x; }
    inline void setY(int y) { this->y = y; }
};

#endif
```

### CMake File

```
cmake_minimum_required(VERSION 2.8)
set(CMAKE_CXX_STANDARD 11)
project(cpp-cmake-circle-ci)

set(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)
add_executable(point_test ${PROJECT_SOURCE_DIR}/src/pointTest.cpp)
```

### Writing Tests for the Point Function

For testing in C++, I like the [Catch2](https://github.com/catchorg/Catch2) library.

#### pointTest.cpp

This is not the final version of the test file.

```c++
#define CATCH_CONFIG_MAIN
#include "libs/catch.hpp"
#include "point.hpp"

TEST_CASE("Creating a Point", "[point]") {
    Point p;

    REQUIRE(p.getX() == 0);
    REQUIRE(p.getY() == 0);
}

TEST_CASE("Creating and Setting Values to a Point", "[point]") {
    Point p;
    p.setX(2);
    p.setY(3);

    REQUIRE(p.getX() == 2);
    REQUIRE(p.getY() == 3);
}
```

### Building using CMake and executing the tests.

![It works!!]({{site.baseurl}}/assets/img/point-test.png)

Yay! Now we have a point class and we succesfully tested it, we can now peacefully create points whenever we want and we will trust the code works because the test said it does, now let's set up the CI!.

### Setting UP the CI

The Conitnuous Integration environment I'm going to use is the [Circle CI](https://circleci.com). I encourage you to visit their website and skim through what it does and on the documentation.

When you are ready, Sign Up and just your GitHub account, it will be easier. Then go to your [GitHub](https://github.com) page and visit the [Marketplace](https://github.com/marketplace), you find *Circle CI* under the Continuous Integration category, otherwise you can just search for it. If you want the direct link [here it is](https://github.com/marketplace/circleci). "Buy" it (it's not buying since it's free).

Now on the CircleCI, Log In if you haven't already, you should be seeing your Dashboard. Add a *New Project* on the *Projects* session and choose your desired Repository, click on Setup Project. Image Below for better understanding.

![Project on Circle CI]({{site.baseurl}}/assets/img/project-on-circle-ci.png)

You will read the following:

> CircleCI helps you ship better code, faster. To kick things off, you'll need to add a config.yml file to your project, and start building. After that, we'll start a new build for you each time someone pushes a new commit.

It comes pretty much all set for you, we usually want to use Linux Operating System and the version 2.0 - For the language, they don't have Official C++ Support, but we can use it anyway. If you want to, you go on *Other* and submit C++ as a language request.

Read carefully what Circle CI is saying to you.

> Create a folder named .circleci and add a fileconfig.yml (so that the filepath be in .circleci/config.yml).

Unfortunately, there isn't a sample `config.yml` file for our project settings, so I will guide through it. If you aren't very familiar with .yml files, I suggest that you [read more](http://yaml.org) about it. Also, if you aren't very familiar with docker or containers, I suggest you [to be](https://www.docker.com). *Please, consider learning more about Docker, we will be using it but we won't be using nearly 1% of its capacities, docker's main page title is: Build, Ship, and Run Any App, Anywhere - Pretty powerful, eh?*

#### Configuring with the config.yml file

For testing in the CI, it is used docker, we are going to use *debian:stretch* docker image, which is a simple Linux Image with few functionalities, so we are going to need to install what we are effectively going to use: **sudo**, **gcc and g++** and, finally, **cmake**.

Every .yml file begins with the current version, which for this case is 2.

`version: 2`

Now we need to define our jobs, a job is a task executed on the continuous integration environment. We will call our job *build* and, for this simple scenario, we are going to do pretty much everything in this job. The build job needs to have a defined docker image and its steps, our steps are:

1. Installing SUDO
2. Installing GCC and G++
3. Installing CMAKE
4. Creating the Build Files
5. Building the Project
6. Executing the point test

*.yml* files are determined by 2-space indentation, so be careful when writing your own.

#### The final config.yml file:
```
version: 2

jobs:
  build:
    docker:
      - image: "debian:stretch"
    steps:
      - checkout
      - run:
          name: Installing SUDO
          command: 'apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*'
      - run:
          name: Installing GCC
          command: 'apt-get update && apt-get install -y gcc g++'
      - run:
          name: Install CMAKE
          command: 'apt-get update && sudo apt-get install -y cmake'
      - run:
          name: Creating Build Files
          command: 'cmake -H. -Bbuild'
      - run:
          name: Creating Binary Files
          command: 'cmake --build build'
      - run:
          name: Point Unit Testing
          command: './bin/point_test'
```


