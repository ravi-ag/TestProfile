---
layout: post
title: Lambda expression comparison between C++11, C++14 and C++17
category: Dev
tags: [C++, C++11, C++14, C++17, Lambda, Lambda expression]
---

In this post I talk about what has been added in the **C++ standard** regarding **lambda expressions** since they were introduced in **C++11**.

**All the code and configuration files used in this post are available in this
[repo](https://github.com/maitesin/blog/tree/master/lambda_comparison_2016_05_14) in GitHub.**

# What is a lambda expression?
A **lambda expression** is a simplified notation for defining and using an anonymous function object. Instead of defining a named class with an *operator()*, later making an object of that class and finally invoking it.[^1]

I do not explain all the options for capturing or specifying return types. There is plenty of material regarding these topics. I focus on what has been introduced in **C++14** and what will be introduced in **C++17**.

# Basics of the lambda expression
The following is the smallest **lambda expression** with its three parts:

- **[]**: capture.
- **()**: parameters.
- **{}**: body.

<script src="https://gist.github.com/maitesin/888fa96e4c331375e21a766f6ca3b0cf.js"></script>

The following **lambda expression** increments by one the parameter.
<script src="https://gist.github.com/maitesin/876ddca2a1d6a599e65de3d6046c9a38.js"></script>

The result of the execution of the previous code is:
<script src="https://gist.github.com/maitesin/b17b1c0250255e77023598135ec105ff.js"></script>

The following **lambda expression** shows the difference between capturing by value **=** and by reference **&**:
<script src="https://gist.github.com/maitesin/152f441e6bb8c7dcf22693d8c265398d.js"></script>

The result of the execution of the previous code is:
<script src="https://gist.github.com/maitesin/e6a65920d2e238a908f34174f4537743.js"></script>

# What has been added in C++14
In **C++14** two new features were added to **lambda expressions**:

- **Initialization captures**: A capture with an initializer acts as if it declares and explicitly captures a variable declared with type auto, whose declarative region is the body of the **lambda expression**.[^2]
- **Generic lambda expressions**: Until **C++14** parameters of a **lambda expression** should be of a specific type. Now, **lambda expressions** accept **auto** as a valid parameter type.

Example of the **initialization captures**:
<script src="https://gist.github.com/maitesin/53cf0003e3376146e917cce0422b0330.js"></script>

The result of the execution of the previous code is:
<script src="https://gist.github.com/maitesin/24765f2cae4e16d07e0e9ac79bf1191e.js"></script>

Example of a **generic lambda expression**:
<script src="https://gist.github.com/maitesin/74ba171ce6e9bfe6b30db7fc3e028796.js"></script>

The result of the execution of the previous code is:
<script src="https://gist.github.com/maitesin/ff9542b04599b35cc5a4ec303818eb7d.js"></script>

# What will be added in C++17
The current plan is to add two new features for **lambda expressions** in **C++17**:

- **Capture &lowast;this**: This will allow the **lambda expression** to capture the **enclosing object by copy**. This will make possible to use safely the **lambda expression** even after the **enclosing object** has been destroyed.
- **constexpr lambda expressions**: This will allow to call **lambda expressions** and use their result to generate ***constexpr*** objects **at compile time**.

Sadly neither [GCC](https://gcc.gnu.org/projects/cxx-status.html#cxx1z) or [Clang](http://clang.llvm.org/cxx_status.html) in any stable version supports them. Therefore, **there will not be any execution of code, but there is some code that should work once the features are implemented**. The information used to do the code of this section has been found in the [C++ reference website](http://en.cppreference.com/w/cpp/language/lambda) and in [the paper for constexpr lambda expressions](https://isocpp.org/files/papers/N4487.pdf).

The following is an example of the **capture &lowast;this**:
<script src="https://gist.github.com/maitesin/adcc64d30ca9ddbb8cea8c7d2466a40f.js"></script>

The following is an example of the **constexpr lambda expression**:
<script src="https://gist.github.com/maitesin/c175630f22c7c8d6aaa16a743c36279a.js"></script>

**Note**: Once **GCC** or **Clang** support these features I will try the code above and I will ammend it if necessary.

[^1]: Definition extracted from the book **The C++ Programming Language (Fourth Edition)**.
[^2]: Definition extracted from the website [cppreference.com](http://en.cppreference.com/w/cpp/language/lambda).
