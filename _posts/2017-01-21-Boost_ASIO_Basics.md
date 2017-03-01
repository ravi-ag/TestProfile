---
layout: post
title: Boost ASIO basics
category: Dev
tags: [C++, Boost, ASIO]
---

**Boost ASIO** library is the *defacto standard* for **network** and **low-level I/O programming**. It has a **great documentation available online**, but there are a lot of methods and classes in the library. Therefore, if it is your first attempt to use it, it can be a bit challenging.

Since there is a lot of ground to cover in the **Boost ASIO** library, I will only cover the **work scheduler** and the **synchronous** methods in this post. The **asynchronous** methods and **timers** will be covered in following posts.

During this post I use **CMake** to configure and build the project and as **dependency manager** I will use **[conan](https://www.conan.io/)**.

As always, **all the code used in this post is available in this [repo](https://github.com/maitesin/blog/tree/master/boost_asio_2017_01_21)**.

The videos are made with **[asciinema](https://asciinema.org/)**, that means **you can copy from the video**.

# IO Service
The **IO Service** is the mainstay of the **Boost ASIO**. Basically, it is in charge of scheduling the work to be done. In this section, I will use the **IO Service** to demonstrate how it performs the scheduling. However, it is not recommended to use it for this purpose, as [**Sean Parent** shows in this talk](https://www.youtube.com/watch?v=QIHy8pXbneI).

**The whole idea of this section is to help you understand how the Boost ASIO works underneath.**

## One worker
The code for this simple example is to just show how the **IO Service** can perform some work.
<script src="https://gist.github.com/maitesin/38a3d7ff61686cceee8adcc18474a4b8.js"></script>
The code is quite straightforward, it provides 6 tasks to the **IO Service** and then it executes them.

### Execution
The output of the previous code is what everybody can expect.
<script type="text/javascript" src="https://asciinema.org/a/1bwz66y6imztewkt2kriaqnjh.js" id="asciicast-1bwz66y6imztewkt2kriaqnjh" async></script>

## Two workers
The code for the two workers is a bit more complicated. The idea is the same, but with two threads executing tasks instead of one.
<script src="https://gist.github.com/maitesin/ebd74168110ffa558c455c3d9f80445d.js"></script>
The complexity of the previous code is the usage of threads, but it is not overwhelming.

### Execution
The output of the *two workers* code can be the same as from the *one worker* section. However, it is not guaranteed that it would be the same. This can be seen in the following execution:
<script type="text/javascript" src="https://asciinema.org/a/da8dspoce5ud1k1a49rm9qlzm.js" id="asciicast-da8dspoce5ud1k1a49rm9qlzm" async></script>

# Synchronous
In order to use **Boost ASIO** for **network** it is not required to know how the **IO Service** works. Actually, the explicit usage of **IO Service** as it is done in the previous section is not recommended.

In this section, the usage of **synchronous** calls is shown in order to create a **client** and a **server** programs.

## Client
The code for the client sends a message - provided as a parameter - to the server. After that the client reads the answer from the server and prints it in the standard output.
<script src="https://gist.github.com/maitesin/ecad73a23af61b4910c291654feac6b5.js"></script>

The first interesting part of the code is the *resolution of the hostname and the port*. Pay attention to the fact that the "port" is a string. It is because it can be a number or a service name (i.e. "https" as port would connect to port 443).
<script src="https://gist.github.com/maitesin/435eaf9c3958e3f9151e7799c0b03feb.js"></script>

The next interesting part of the code is the *send of the message to the end point*. The interesting bit is how the data is send. In this case, the message is stored in a *std::string*, but it is required to be transformed into a *boost::asio::buffer*.
<script src="https://gist.github.com/maitesin/3293a521a7dfa69c1e72e180a3e473d8.js"></script>

The last interesting part of the code is the *read a message from the end point*. In this case, a *boost::asio::streambuf* is used to retrieve the data from the server. However, it could have been done in other ways as it is shown in the **server** case. The errors are another interesting part of the code.  The "success" error (when the first part of the conditional is false) will happen when the buffer is full. However, the *boost::asio::error::eof* will occur when the end point closes the connection.
<script src="https://gist.github.com/maitesin/af9140aefeca0b141f6a73c4711880eb.js"></script>

## Server
The code for the server receives a message, then it returns the message backwards to the sender.
<script src="https://gist.github.com/maitesin/7bd1c4316a150781e1533226cb86feeb.js"></script>

The first interesting part of the code is the *socket and acceptor*. In this case the port is a number. Another good part is how the usage of IPv4 only has been specified.
<script src="https://gist.github.com/maitesin/8ab17b5eeaf15d0a5c9f4914a59bc273.js"></script>

The last interesting part of the code is the *read a message*. In this case, it is using a plain *char* array to read parts of the message and store it in a *std::ostringstream*. Pay attention to how it keeps track of the amount of data that has been read and stores that much information only.
<script src="https://gist.github.com/maitesin/be4ae9a7e2cbcc270ad2c2aa741126b8.js"></script>

## Execution
<script type="text/javascript" src="https://asciinema.org/a/10bf5yr6qyuqh5oxm2ru65qlr.js" id="asciicast-10bf5yr6qyuqh5oxm2ru65qlr" async></script>

# Conclusion
**The simplicity of the Boost ASIO is matched only by its power**. It is quite simple to create a **synchronous** **client/server** application. These are the basics of **Boost ASIO** and it should allow you to understand more complex usage of it.
