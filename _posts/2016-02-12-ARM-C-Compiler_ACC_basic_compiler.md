---
layout: post
title: ARM C Compiler (ACC) - Basic Compiler I 
category: Dev
tags: [C, ARM, Compiler]
---

# Why do you want to create your own compiler?
To answer this question I have to give you some background. For Christmas I got a **BeagleBone Black**, perfect to **learn ARM assembly**. After a few weeks of doing the usual stuff I decided I wanted a bigger project to improve my knowledge. However, the idea to start the **development of a compiler** comes from [http://www.sigbus.info/how-i-wrote-a-self-hosting-c-compiler-in-40-days.html](http://www.sigbus.info/how-i-wrote-a-self-hosting-c-compiler-in-40-days.html).

**The only aim of this project is to improve my knowledge of the ARM assembly, the C language and Compilers**. The project can be found in [GitHub](https://github.com/maitesin/acc).

# What is the target of the project?
My target is to have a **self-hosting compiler of C**. For this reason I want to **keep it simple**. Moreover, I will only develop the features I need to accomplish the target of the project. As you can imagine the choosen language is C (ANSI C to be specific).

I decided **not to use Flex, Bison, Lex or Yacc** because I want to work in all stages of the compiler. Yes, it sounds crazy and maybe I will regret it in the future, but at least I will try it.

# What is the current state of the project?
Currently, I have implemented a **very basic compiler**, this version is available in [GitHub as v0.1](https://github.com/maitesin/acc/tree/v0.1). *It just generates code for a file containing the main function without parameters and the body only contains a return statement of a positive integer*.

## Structure of the project
The project contains three folders:

* *inc*: holds the external libraries needed. Right now, it has the files of **Unity** for unit testing.
* *unittest*: has the unit tests for the project. It has just one, but once I find a good framework to do mocks in C, it will have more unit tests.
* *src*: contains the actual source code of the project.

This compiler is made of a **Lexer** that creates **Tokens**. These **Tokens** are used by the **Grammar** to create **AST nodes**. Finally, the **AST nodes** will be used by the **Generator** to generate the assembly.

### Lexer and Tokens
The available **Tokens** are: *int_value*, *int_type*, *function*, *open_parenthesis*, *close_parenthesis*, *open_bracket*, *close_bracket*, *return*, *semicolon* and *end_of_file*. These can be found in the file *tokens.h*:
<script src="https://gist.github.com/maitesin/6283d08c5e7694ab2b50.js"></script>
These **Tokens** are returned by the method *next* from the **Lexer** in the file *lexer.h*:
<script src="https://gist.github.com/maitesin/f6676ea0ad3ca78da248.js"></script>

### Grammar and AST nodes
The available **AST nodes** are: *id*, *int*, *function* and *return*. These can be found in the file *ast.h*:
<script src="https://gist.github.com/maitesin/9b2260be200671f2accd.js"></script>
The **AST** is returned by the method *build_ast* from the **Grammar** in the file *grammar.h*:
<script src="https://gist.github.com/maitesin/ce3c7befe3c129421263.js"></script>

### Generation of assembly
The **Generator** has a **Grammar** that will be used to generate the assembly with the method *generate_code*:
<script src="https://gist.github.com/maitesin/9c78f8c245a314e1eb59.js"></script>

# Example of the current functionality:
The code to be compiled into ARM assembly is:
<script src="https://gist.github.com/maitesin/b51632d6301aeff40a49.js"></script>
Compile the example with our compiler (ACC):
<script src="https://gist.github.com/maitesin/4f457fbfb6e5d49b8725.js"></script>
The assembly generated is:
<script src="https://gist.github.com/maitesin/dd13d3b06fcc75105123.js"></script>
Use GCC to translate that assembly into a executable binary:
<script src="https://gist.github.com/maitesin/00956faf80a4c99d1840.js"></script>
Execute and check the result:
<script src="https://gist.github.com/maitesin/561abfc3e2e78778362a.js"></script>

# Future
As you can see this project is going to take a long time to be completed. My plan is to work adding small features at a time. The next step is to add **conditionals (if and else)**. Afterwards, I will add **operators such as <, <=, >, >=, == and !=**. **Everytime I achieve a new goal in this project I will create a post like this one to show the feature and how it has been implemented**. 
