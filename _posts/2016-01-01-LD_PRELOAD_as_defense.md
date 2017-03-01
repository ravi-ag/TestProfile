---
layout: post
title: Using LD_PRELOAD as defense from unsecure library calls 
category: Linux
tags: [Security, Linux]
---

I have seen LD_PRELOAD used in several cases. From using it to *allow programs that link to a newer version of the libstdc++*, to *cracks for applications that hijack some calls and provide the expected result to tell the application they have a valid license*.

**The aim of this post is to show how to find these dangerous calls in applications that you are running which you cannot fix (i.e. you do not have access to the source code).** Imagine that one of these applications uses the library call *strcpy*, as we know that call is dangerous and we have more secure alternatives such as *strncpy*.

**All the code used in this post is available in this [repo](https://github.com/maitesin/blog/tree/master/ld_preload_2016_01_01/src)**

As an example of these applications I will use the following code:
<script src="https://gist.github.com/maitesin/ff9489767218d3c55a95.js"></script>

<script src="https://gist.github.com/maitesin/de99f58fbe1ea5f08e4b.js"></script>

As you can imagine, we will have problems if the user gives a string longer than 511 characters to the application because it will
override more than just the string. Actually, it can even be an entry point for an exploit (it depends on the compiling flags).

<script src="https://gist.github.com/maitesin/94bf40572e6bdc297f08.js"></script>

# Find which dangerous calls are happenning in the application
To help us in this task we will use **ltrace**. Remember, we do not have access to the code.
<script src="https://gist.github.com/maitesin/552c7fff34f4e494e0ff.js"></script>

In the output above we can see that the call *strcpy* is used. It is dangerous, so we want to use the call *strncpy* instead.

# Using a more secure call than *strcpy*
We can create our own version of the *strcpy* that actually calls *strncpy*:
<script src="https://gist.github.com/maitesin/1271a761a0d7507b10a2.js"></script>

Now we have to compile it as a shared object (library to link):
<script src="https://gist.github.com/maitesin/9f499220972363936183.js"></script>

# Using LD_PRELOAD to call our *strcpy*
LD_PRELOAD will be used to load out *strcpy* instead of the one provided by the standard library.
<script src="https://gist.github.com/maitesin/8d81d78f0348b56105ef.js"></script>

Note that it only copies up to the first 511 characters of the string s2.

# Conclusion
I want to point out the versatility of the LD_PRELOAD, for example think about how can this help mitigate a 0-day exploit until the code is fixed.

This use of LD_PRELOAD is quite common in competitions like a [CTF](https://en.wikipedia.org/wiki/Capture_the_flag#Computer_security) (in the attach/defense style) where you are provided with a server (with some services running) and you have to keep alive your services as much time as you can, but usually the services are an older version with known issues you need to patch on the fly ;)
