---
layout: post
title: ARM C Compiler (ACC) - Basic Compiler II
category: Dev
tags: [C, ARM, Compiler]
---

# Introduction
I am working on a pet project to create a **C compiler for the ARM architecture**. You can find more information about this topic in my previous post [ARM C Compiler (ACC) - Basic Compiler I](http://maitesin.github.io//ARM-C-Compiler_ACC_basic_compiler/).

The source code of this project can be found in [GitHub](https://github.com/maitesin/acc).

# What is the current state of the project?
Currently, I have implemented a **very basic compiler**, this version is available in [GitHub as v0.2](https://github.com/maitesin/acc/tree/v0.2). *Basic compiler able to handle a single file with a main function (without parameters) and if/else statements with simple boolean expressions and return statements of a positive integer*.

## What has been added or modified since the last post?
The difference between both versions can be found in the following link [diff v0.1 v0.2](https://github.com/maitesin/acc/compare/v0.1...v0.2).

### Unit tests
In the previous version only **unit tests** were available for the **Lexer**, but in this new version some **unit tests** for the **Lexer** have been added and **unit tests** for the **Grammar** have been created. In total, **there are 27 (26 new) unit tests for Lexer and 12 unit tests for Grammar**.

### Lexer and Tokens
There are new **Tokens** for **if**, **else** and **boolean operators**. **Lexer** now supports all these new tokens and it has a new feature (a **stack**) that allows the **Grammar** to give a **Token** back to **Lexer**. **That is useful when you are reading a boolean expression, but you do not know if it will be a binary or unary expression**. Then in case you try to check if it is a binary expression and it is not, you give back that token to the **Lexer** and try with the unary expression. Moreover, the **Lexer** now receives a buffer with the content of the file loaded instead of a file. This is to make it easier to create **unit tests**.
<script src="https://gist.github.com/maitesin/5865a63bdce61743c19ee712f0d0e443.js"></script>

### Grammar and AST nodes
The most important features added to the **AST** is the addition of **node_if**, **node_boolean_operator** and **enum boolean_operator_type**. In addition to this, now the base of the **AST** holds a pointer to the next **AST** node. **That is to hold the whole information contained in the body of a function or an if statement**.
<script src="https://gist.github.com/maitesin/d4e871c33d937c3fed3b61b0e48a888b.js"></script>
Regarding the **Grammar**, the method **read_function_body** has been refactored into the method **read_body** to be able to re-use it to read the body of the **if** and **else** statements. Another interesting piece of code is the method **read_boolean_expression**, it allows to build a valid **AST** for a complex boolean expression such as *1 <= 2 && 4 == 4*.
<script src="https://gist.github.com/maitesin/2eb7ef7fe589772d7b15694bc895ef59.js"></script>

### Assembly Generator
The **Generator** of the ARM assembly has new methods to handle all the new **AST** structures and behaviours. **It has some limitations regarding boolean expressions in which I have to do further research**. Some pieces of the **Generator** are the following:
<script src="https://gist.github.com/maitesin/304eb5c822b86efd49091e9d2adcf57c.js"></script>

# Example of the current functionality:
The code to be compiled into ARM assembly is:
<script src="https://gist.github.com/maitesin/05afafa443c41042078448efe9c42367.js"></script>
Compile the example with our compiler (ACC):
<script src="https://gist.github.com/maitesin/7b0e845c9898ba70e3816ae3dc1dba57.js"></script>
The assembly generated is:
<script src="https://gist.github.com/maitesin/e2a261eefda5862da26f8e7402a3109d.js"></script>
Use GCC to translate that assembly into a executable binary:
<script src="https://gist.github.com/maitesin/a26e3a2f34b4666bbb6eec36bc3a4368.js"></script>
Execute and check the result:
<script src="https://gist.github.com/maitesin/58d3a94538420cd2fd437a2d4276262d.js"></script>

# Future
The next step will be to actually be able to generate any possible boolean expression. Currently, the **AST** can recognise complex boolean expressions, but the **Generator** is not able to handle them. I have to study and research more about this topic in the **ARM** architecture. After that, **I plan on adding variables (integers)**.
