---
layout: post
title: Clang sanitizers
category: Dev
tags: [C, C++, Clang]
---

**[Clang](http://clang.llvm.org/)** is a compiler front end for the C, C++, Objective-C and Objective-C++ programming languages. It uses [LLVM](http://llvm.org/) as its back end. In this post I talk about some of the **sanitizers** available in **Clang** (some are avilable in **GCC** as well). **They help you detect problems at run time (dynamic analysis).**

As usual, I am working from an Arch Linux computer. Therefore, I can install **Clang** and the tools from the repository (clang). For other distributions you can find the information in the documentation.

As always, **all the code used in this post is available in this [repo](https://github.com/maitesin/blog/tree/master/clang_sanitizers_2016_07_13)**.

The videos are made with **[asciinema](https://asciinema.org/)**, that means **you can copy from the video**.

# Clang sanitizers
The **Clang** sanitizers available are:

* **AddressSanitizer**: detects memory errors. [http://clang.llvm.org/docs/AddressSanitizer.html](http://clang.llvm.org/docs/AddressSanitizer.html)
* **ThreadSanitizer**: detects data races. [http://clang.llvm.org/docs/ThreadSanitizer.html](http://clang.llvm.org/docs/ThreadSanitizer.html)
* **MemorySanitizer**: detects uninitialized reads. [http://clang.llvm.org/docs/MemorySanitizer.html](http://clang.llvm.org/docs/MemorySanitizer.html)
* **UndefinedBehaviorSanitizer**: detects undefined behavior. [http://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html](http://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html)
* **DataFlowSanitizer**: is a generalised dynamic data flow analysis. Unlike other **sanitizers** this one is not designed to detect a specific class of bugs on its own, it provides a generic dynamic data flow analysis framework to be used by clients to help detect application-specific issues within their own code. [http://clang.llvm.org/docs/DataFlowSanitizer.html](http://clang.llvm.org/docs/DataFlowSanitizer.html)
* **LeakSanitizer**: detects memory leaks. It can be combined with **AddressSanitizer**. [http://clang.llvm.org/docs/LeakSanitizer.html](http://clang.llvm.org/docs/LeakSanitizer.html)

In this post I show **AddressSanitizer**, **ThreadSanitizer**, **MemorySanitizer** and **UndefinedBehaviorSanitizer**. I do not talk about **DataFlowSanitizer** because it is a work in progress and it is really specific of the application where you need it. Moreover, I do not talk about **LeakSanitizzer** because it is a subset of checks from **AddressSanitizer** that can be run independently from it.

## AddressSanitizer

### Usage of freed memory
The code below is trying to use a region of memory that has been freed already.
<script src="https://gist.github.com/maitesin/9691a52866d7ed9f890fe44740bd9cba.js"></script>
We can compile it with:
<script src="https://gist.github.com/maitesin/86b8950c006596c9559aa2e3ffce1cf2.js"></script>
When you run the previously generated executable you will get something similar to the following:
<script type="text/javascript" src="https://asciinema.org/a/3zcpyg71hz6sxnhhxru7pvgtj.js" id="asciicast-3zcpyg71hz6sxnhhxru7pvgtj" async></script>

### Buffer overflow
The code below is trying to access a memory region that is not part of the allocated one.
<script src="https://gist.github.com/maitesin/5165ec922b929e15cd665ee8f7543eb0.js"></script>
We can compile it with:
<script src="https://gist.github.com/maitesin/a5b5e1332bc82ea9be35fce13f2b24a0.js"></script>
When you run the previously generated executable you will get something similar to the following:
<script type="text/javascript" src="https://asciinema.org/a/c0338bklzn84ptgafgaj4kas3.js" id="asciicast-c0338bklzn84ptgafgaj4kas3" async></script>

### Memory leak
The code below is allocating memory twice but it only frees the memory once.
<script src="https://gist.github.com/maitesin/b8695f567e2ed171b1ec95ca55b4c986.js"></script>
We can compile it with:
<script src="https://gist.github.com/maitesin/c16aa44874247d26e9e962fa787bfec2.js"></script>
When you run the previously generated executable you will get something similar to the following:
<script type="text/javascript" src="https://asciinema.org/a/91kmpmy03843ccdbbh04ptbdd.js" id="asciicast-91kmpmy03843ccdbbh04ptbdd" async></script>

### Double free
The code below is allocating memory once but it is freeing it twice.
<script src="https://gist.github.com/maitesin/72c56cba930f0470e217989b5aa25a3d.js"></script>
We can compile it with:
<script src="https://gist.github.com/maitesin/03b22be564adb526ddef9cdeb6a844f5.js"></script>
When you run the previously generated executable you will get something similar to the following:
<script type="text/javascript" src="https://asciinema.org/a/ebgp9ox48e8ffdaf0iug0b37s.js" id="asciicast-ebgp9ox48e8ffdaf0iug0b37s" async></script>

## ThreadSanitizer

### Data race
The code below has a data race due to having two threads modifying the same global variable.
<script src="https://gist.github.com/maitesin/1769638fae0ec18de72e675884bf6ce1.js"></script>
We can compile it with:
<script src="https://gist.github.com/maitesin/9c25efc03ecf8962d09de3fc72779acf.js"></script>
When you run the previously generated executable you will get something similar to the following:
<script type="text/javascript" src="https://asciinema.org/a/5go47caz8s1t6mdsaexb10ecx.js" id="asciicast-5go47caz8s1t6mdsaexb10ecx" async></script>

## MemorySanitizer

### Uninitialized values
The code below is reading the value stored in an array that has not been initialized.
<script src="https://gist.github.com/maitesin/28b74b40b956c005bc3ba603a031581c.js"></script>
We can compile it with:
<script src="https://gist.github.com/maitesin/a6acf4e267c99010d2d1bbfa122333ab.js"></script>
When you run the previously generated executable you will get something similar to the following:
<script type="text/javascript" src="https://asciinema.org/a/enrqyt3iue9lvmixapugzljge.js" id="asciicast-enrqyt3iue9lvmixapugzljge" async></script>

## UndefinedBehaviorSanitizer

### Function not returning a value
The code below contains a function that must return an integer, but it does not.
<script src="https://gist.github.com/maitesin/8928ea5485a19953e0a7c77baa5bc6cf.js"></script>
We can compile it with:
<script src="https://gist.github.com/maitesin/5338468c144852782ecb892d4695ce95.js"></script>
When you run the previously generated executable you will get something similar to the following:
<script type="text/javascript" src="https://asciinema.org/a/bqcprr20fga4yrdyq69vs3aek.js" id="asciicast-bqcprr20fga4yrdyq69vs3aek" async></script>


# Conclusion
Using these tools to compile your code **to run your tests (integration test, smoke test, system test, etc) helps you catch plenty of problems**. The downside is that you **cannot use two sanitizers at the same time** (except **AddressSanitizer** and **LeakSanitizer**). Therefore, you need multiple binaries to test your code with all these sanitizers, but the payoff is worth it.
