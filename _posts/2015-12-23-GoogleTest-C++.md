---
layout: post
title: Unit test with Google Test for C++ (Improved version)
category: Dev
tags: [C++, Google Test, Unit Test]
---

**Google Test** is one of the available Frameworks to create **unit test** for C++. In this example I will use **CMake**
to configure the project and build. Furthermore, for the **dependency manager** I will use the new and shiny
**[conan](https://www.conan.io/)**.

Before starting, why use a **dependency manager** such as conan or software to configure and build such as **CMake**?
Because these technologies are widely use it in real projects.

**All the code and configuration files used in this post are available in this
[repo](https://github.com/maitesin/blog/tree/master/google_test_2015_12_22) in GitHub.**
# Step 1 Install conan, configure project and gather dependencies
First of all we need to have install conan from pip2 doing:
<script src="https://gist.github.com/maitesin/d8d33eca7ceb3edcbef6.js"></script>

Now with conan installed we do not have to worry about installing **Google Test** in our system.

Next step will be preparing the **conanfile.txt** to gather de dependencies:
<script src="https://gist.github.com/maitesin/7938012180e0cf320a55.js"></script>

Once we have conan ready we only need to run it to download the dependency and configure them to have them ready for
CMake:
<script src="https://gist.github.com/maitesin/81ac8f571bacd69bd718.js"></script>
This will output something similar to:
<script src="https://gist.github.com/maitesin/3714993e356504563c91.js"></script>

# Step 2 configuring CMake
The configuration will be done in the **CMakeLists.txt** file:
<script src="https://gist.github.com/maitesin/2c1b975085281d016e9d.js"></script>

# Step 3 code and unit test
The code will be held in the **src** folder. It will contain two files: *functions.h* and *test.cpp*.

The function(s) we want to test will be in the header file *function.h*:
<script src="https://gist.github.com/maitesin/224e6c2814c2f3ebdee6.js"></script>


The test(s) we want to run will be in the source file *test.cpp*:
<script src="https://gist.github.com/maitesin/2c1c3e121c45c9950c69.js"></script>

# Step 4 putting all together
What is left to do is actually build the project and run the test. In order to do this we need to run:
<script src="https://gist.github.com/maitesin/8b1498ae67bf305c6ede.js"></script>

This will output something similar to:
<script src="https://gist.github.com/maitesin/358be24668b54d1f22a4.js"></script>

There will be now a **Makefile** generated from **CMake** with everything ready to compile and link all the sources and
dependencies together.
<script src="https://gist.github.com/maitesin/72bf69c72b4877f6de4e.js"></script>

This will output something like:
<script src="https://gist.github.com/maitesin/80a9faf26be9d3a1901d.js"></script>

This will generate an executable in the *bin* folder, and we will be able to run them with the command:
<script src="https://gist.github.com/maitesin/dd4cc2237a83a0776f0d.js"></script>

This will result with the following output:
<script src="https://gist.github.com/maitesin/6f5d11514f8c389d3915.js"></script>

# More advanced example
These are the basics of how to use **Google Test** to create **unit test** for your application. In the website of the project there are plenty of more advanced examples.

Finally, if you want to see how is used **Google Test** in one of my own project you can have a look to the repository [tries](https://github.com/maitesin/tries). For each of the three data structures (Trie, TST and Radix Tree) there are two folders: **lib** (where the source code of the data structure is stored) and **gtest** (where the unit test are stored).
