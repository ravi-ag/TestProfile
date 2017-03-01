---
layout: post
title: FFF a mocking frameowork for C 
category: Dev
tags: [C, Unit Test, Mock]
---

This post is a continuation from a previous post called [Unity; unit test for C](http://maitesin.github.io//Unity_unit_test_for_C/), but in this post we are going to use **FFF**.

**FFF** is one of the available mocking frameworks for C. In this example I will use **CMake** to configure the project and build it.

**All the code and configuration files used in this post are available in this [repo](https://github.com/maitesin/blog/tree/master/fff_mock_2016_02_18) in GitHub.**


# Why do we need to mock functions?
To answer that question I will introduce the signature of the two methods I will use during this post.

The first function is called *modulo* and it will return 0 if the given number is even, 1 if it is an odd number:
<script src="https://gist.github.com/maitesin/02b62c9a46897b0f3569.js"></script>

The second function is called *both_even* and it will return 0 if both numbers are even, otherwise:
<script src="https://gist.github.com/maitesin/75ef1d9612086f1ac1da.js"></script>

Finally, the implementation of the *both_even* is the following:
<script src="https://gist.github.com/maitesin/62376e08fe992dfa9f9c.js"></script>

In this case we will use the *modulo* function to calculate *both_even*, but we do not have the implementation of *modulo*. This is intended because we do not want to use the actual code of the *modulo* function. We want to test **only the code in *both_even***. Therefore, the *modulo* function will be mocked.


# Unit test with FFF
The following code is an example of 6 **unit tests** written using the **FFF** framework:
<script src="https://gist.github.com/maitesin/67b9fdfe9ede1cc6aca5.js"></script>

On the one hand, the beginning of the code is the initialization of the **FFF** framework and a couple of definitions that will come in handy:
<script src="https://gist.github.com/maitesin/98ad57f150beac4396bd.js"></script>

However, the interesting part is:
<script src="https://gist.github.com/maitesin/b4034745090c5d190e3e.js"></script>
The *macro* *FAKE_VALUE_FUNC1* receives three parameters, first the type of the return, second the name of the function to mock and third the type of the parameter. You can find all the information about this in the [documentation](https://github.com/meekrosoft/fff).

On the other hand, each test looks as follows:
<script src="https://gist.github.com/maitesin/98c04a8b454ff9cfdfa2.js"></script>
And the interesting piece is under the *Given* part. Note that we are configuring the *modulo_fake* struct that contains the information that the *modulo* function will return. Actually, we are saying that it will be called twice and which will be the return values.

The execution of the test will be the following:
<script src="https://gist.github.com/maitesin/deb6e29d5d52fb1fa46c.js"></script>


# Conclusion
**FFF**, or any mocking framework, is an important tool for developers because **it allows to create unit tests for interoperability of different functions**. Furthermore, you can create **unit tests** that are focused on a single function without worring if the functions to which it depends are working properly.
