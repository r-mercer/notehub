# Modern C - Encounter

## Introduction

### C && C++

It might have been true at some stage to say that C++ was just as superset of C, however thish as not been the case for over 30 years. There are some similarities and cross polination, however in general they are distinct languages.

### Don't Panic

Programming in C is about having the computer complete some specific tasks.

A C program does this by giving orders, in imperative language.

### Imperative Programming

The orders that we give the copmuter to execute. Provides a link between us and the machine.

### Compilers

C is a compiled language, which means that it must be compiled into a different binary depending on which platform that is being used. Generally this is done by a compiler on that platform.

This is generally the point of C, it is an abstraction above platform specific assembly language which allows the common Language (c) to be compiled into different languages for different systems.

good c code will generally be portable. However some things will ruin your day and some platforms do not truly implement C fully.

> [!NOTE]
> Portable meaning, it will compile into an executable with the same behaviour on different platforms.

`Exercise 3 Notes` 
`- return code is wong, no imports, did we declare int out of scope?` 

## The Principal Structure of a Program

There are two main aspects to consider in a C program:
Syntactical, how we specify the program so the compiler understands, and
Semantic, what do we specify so the program does what we want it to do.

### Syntactical

Grammar is the main syntactical aspect to discuss. This includes Special words, or aspects of the language that cannot be changed,anged. Punctuation, such as structural text (brackets and commas), comments, literals, identifiers (for data objects, type aliases functions and constants), functions, operators and attributes.

### Semantic

We shall discuss three semantic aspects of C, declarative parts, definitions of objects and statements.

literals are the literal values represented by vars. 

Identifiers are the var names as well as type aliases, function names and constants. All identifiers in a program must be declared.

Declarations start with a type and then generally a value.

Easy test to distinguish between the two is that Declarations specify identifiers, where as definitions specify objects. So you declare x is an int, then you define x as 2.


