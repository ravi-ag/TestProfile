---
layout: post
title: Unit test with Google Mock for C++
category: Dev
tags: [C++, Google Test, Google Mock, Unit Test, Mock]
---

This post is a continuation from a previous post called [Unit test with Google Test for C++](http://maitesin.github.io//GoogleTest-C++/), but in this post we are going to use **Google Mock**, that extends the functionality of **Google Test**.


**Google Mock** is one of the available frameworks for C++ to mock objects in **unit tests**. In this example I will use the same technologies as for the previous one: **CMake** to configure the project and build it, for the **dependency manager** I will use the new and shiny **[conan](https://www.conan.io/)**.


**All the code and configuration files used in this post are available in this [repo](https://github.com/maitesin/blog/tree/master/google_mock_2016_01_22) in GitHub.**

# Why do we need to mock objects?
To answer that question I will introduce the two classes I will use during this post.


The first class is called *Producer* and it will extract the domain of a given URL:
<script src="https://gist.github.com/maitesin/9162d164f6bcadbe2384.js"></script>


The second class is called *Consumer* and it will calculate the level of the domain of a given URL:
<script src="https://gist.github.com/maitesin/1fbfc223814834e67439.js"></script>

In this case we want to create a unit test for the *Consumer* class, but we need to implement a *Producer* class for that, right? Well, no. **We do not need an implementation of the *Producer* class because we can mock it**. Moreover, in that case the unit test would not be only for the *Consumer* class, but for the *Producer* too, and that is not what we are looking for.


# Unit test with Google Mock
The following unit test is written using the **Google Mock** framework:
<script src="https://gist.github.com/maitesin/6ec71be17fde199e4ab3.js"></script>

On the one hand, the *ProducerMock* class is inheriting from the *Producer* class. However, the interesting part is
<script src="https://gist.github.com/maitesin/f37c379a6735e719dcd5.js"></script>
The line above is telling **Google Mock** to mock the method *getDomainFromUrl* from the *Producer* class and it will receive a *const std::string* by reference as parameter and it will return a *std::string* by value.


On the other hand, there is the *EXPECT_CALL* macro that receives two parameters:
<script src="https://gist.github.com/maitesin/7fa2cae3d7cfbe726bf0.js"></script>
The first parameter is the mocked object and second one is the method that will be called and **the parameter that will be passed**. The result of the expansion of the macro will call a method called *WillOnce* that will make the mocked object answer once to the method. Finally, *::testin::Return(domain)* specifies the return of that call, which will be *domain*.


The execution of the test will be the following:
<script src="https://gist.github.com/maitesin/70e1d164d358cb786d52.js"></script>

# Conclusion
**Google Mock**, or any mocking framework, is an important tool for developers because **it allows to create unit tests for interoperability of different objects**. Furthermore, you can create **unit tests** that are focused on a single class without worring if the class to which it depends is working properly.
