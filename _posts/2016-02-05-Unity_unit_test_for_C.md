---
layout: post
title: Unity; unit test for C
category: Dev
tags: [C, Unity, Unit Test]
---

**Unity** is one of the available frameworks to create **unit tests** for C. In this example, I will use **CMake** to configure the project and build.

**All the code and configuration files used in this post are available in this
[repo](https://github.com/maitesin/blog/tree/master/unity_c_unit_test_2016_02_05) in GitHub.**

# Can you do unit test in C? And what is Unity?
**Some people do not know you can do unit tests in C** and people must do **unit tests** in any language. My choice to do **unit tests** in C is **Unity** for several reasons:

 * No need to install any package in your distro. Just add the three files to your project and start using it.
 * Great documentation and examples in the [repo](https://github.com/ThrowTheSwitch/Unity) and their [website](http://www.throwtheswitch.org/).
 * Awesome course about **unit test** in **embedded systems** on [Udemy](https://www.udemy.com/unit-testing-and-other-embedded-software-catalysts/). I did it myself, totally worth it if you are interested in these topics.

# Structure of the project
In the root of the project we have the **CMakeLists.txt** file and three folders: *lib*, *src* and *unittest*.

 * *lib*: Contains the **Unity** files.
 * *src*: Contains the code we want to test. In this case, a functions header file.
 * *unittest*: Contains the test we want to run.
<script src="https://gist.github.com/maitesin/3cb2890c4684f6651446.js"></script>

# The project to test
The configuration will be done in the **CMakeLists.txt** file:
<script src="https://gist.github.com/maitesin/a26c6aafb5abb2964198.js"></script>

The code we want to test is:
<script src="https://gist.github.com/maitesin/1a676045b4a87e8a2cc2.js"></script>

Finally, the **unit test**:
<script src="https://gist.github.com/maitesin/4e838ead62f97a149636.js"></script>
As you can see in the code above, you need a main in the **unit test** file that contains: *UNITY_BEGIN* and *UNITY_END* calls and a *RUN_TEST* call for each test we want to run.

# Conclusion
**Everybody should be doing unit tests, it does not matter what language do they use**. I do use this framework among others (that I will introduce in future posts) for testing C applications. **Unity** is a must have tool for any C developer, **even for embedded software**. You can check it in their website and course.
