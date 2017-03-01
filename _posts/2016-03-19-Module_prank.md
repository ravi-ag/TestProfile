---
layout: post
title: Linux Kernel Module example. Rickroll prank.
category: Linux Kernel
tags: [C, Module, Linux Kernel]
---

**NOTE: The code used to replace the user's path with the one provided is BAD, never change user's pointer content unless he/she is expecting that to happen. Don't do that at home kids**

I decided to explain the basics of a **Linux Kernel Module** with humor. I am not saying this is a good idea for April's fool, but it is quite close ;)

This module shares some ideas with the post about [LD_PRELOAD](http://maitesin.github.io//LD_PRELOAD_as_defense/), but this time it is not to defend ourselves. **The module will replace the open syscall for our own where it will detect if we are opening an mp3 or a jpg file**. This idea was taken from [this talk of Julia Evans](https://www.youtube.com/watch?v=0IQlpFWTFbM).

As always, **all the code used in this post is available in this [repo](https://github.com/maitesin/blog/tree/master/rickroll_module_2016_03_19)**.

# Skeleton of a Linux Kernel Module
The following code is the skeleton of a **Linux Kernel Module**
<script src="https://gist.github.com/maitesin/6bfc964fa1fe1e816494.js"></script>

I think the code above is quite self-explanatory.

# What does this Module do?
As said before, this module is aimed to replace the current open syscall by ours, that will detect if the file we are trying to open is an mp3 or a jpg file and it will substitute the files by the ones provided when the module is loaded.

## Code explained
This could seem a bit overwhelming, but let's go through it.
<script src="https://gist.github.com/maitesin/461b74a13f1d1ca34cf6.js"></script>

Parameters provided to the module are declared and registered as follows:
<script src="https://gist.github.com/maitesin/481eba1e505c92e27895.js"></script>

Pointer to the old syscall and declaration of the new one. Moreover, we can see the method ***copy_to_user***, this is to copy information from the **Kernel Space** to **User Space**.
<script src="https://gist.github.com/maitesin/a175d0f01e22d4606960.js"></script>

The most outstanding part of the following piece of code is the use of ***kallsyms_lookup_name*** function. We will use it to locate the address of the ***sys_call_table***.
<script src="https://gist.github.com/maitesin/7e857ba5bdb3f8e900d3.js"></script>

**Note: the *sys_call_table* is in read-only mode by default. To be able to write on it, the kernel running must have been compiled with the flag *CONFIG_DEBUG_RODATA* not set.**

# Example of how it works
<iframe width="560" height="315" src="https://www.youtube.com/embed/efEZZZf_nTc" frameborder="0" allowfullscreen></iframe>

# Conclusion
This is a great and funny example of the power of the **Linux Kernel Modules**. I will write about more advanced examples in the future. Moreover, I will keep on trying to get a patch accepted in the main tree :D
