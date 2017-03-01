---
layout: post
title: rr debugger, record once, replay tens of times
category: Dev
tags: [C, C++, Debugger, rr, GDB]
---

**rr** is a debugging tool from Mozilla that **enhances the behaviour of GDB**. It can be found in
[GitHub](https://github.com/mozilla/rr), but my recomendation is to go first to the website of the
[project](http://rr-project.org/). In the website you can find really useful information and documentation.

As usual, I am working from an Arch Linux computer. Therefore, I can install **rr** from the AUR repository (rr). For other distributions you can find the information in the documentation.

As always, **all the code used in this post is available in this [repo](https://github.com/maitesin/blog/tree/master/rr_debugger_gdb_post_2016_01_15/src)**.

The videos are made with **[asciinema](https://asciinema.org/)**, that means **you can copy from the video**.

# Before starting debugging
It is recommended to have your CPU frequency governor in 'performance' mode instead of 'powersave'. Because of that, it is recommended to set it:
<script src="https://gist.github.com/maitesin/93a7e3ad10d8afba3529.js"></script>


To set it back to 'powersave' mode just run:
<script src="https://gist.github.com/maitesin/83cadbdedf5e3a6925a2.js"></script>

# How **rr** works
**rr** works in two phases. In the first one, it *records* the execution of a program. In the second one, it *replays* the execution of that program as many times as you need. Moreover, during the *replay* phase **you can go forward and backward in the execution**.

**rr** adds some new commands to **GDB** such as *reverse-next* or *reverse-continue*. I think these commands do not need more explanation.

# Example of usage
In this example I will use the following code:
<script src="https://gist.github.com/maitesin/efdbc9067edb3d5871e3.js"></script>


It will be compiled with the *-g* to produce debugging information as you would do for **GDB**:
<script src="https://gist.github.com/maitesin/fd19939785d85babda39.js"></script>

## Record of the execution
In the following video it will be shown how to record the execution of an application.
<script type="text/javascript" src="https://asciinema.org/a/5m0lpbkqj6xyl9fy0ath9tnjd.js"
id="asciicast-5m0lpbkqj6xyl9fy0ath9tnjd" async></script>

## Replay of the execution
In the folowwing video it will be shown how to replay the execution of an application.
<script type="text/javascript" src="https://asciinema.org/a/cpzdimjm3v3ghownpynzey1bu.js"
id="asciicast-cpzdimjm3v3ghownpynzey1bu" async></script>

# Conclusion
Personally, I consider **rr** a **must have** tool for debugging. The ability to go backwards in the execution at any time is extremely useful when you are trying to find the exact moment where something starts to go wrong. Moreover, the fact that you do not have to keep providing any kind of input to the app is really good to focus just in the execution.
