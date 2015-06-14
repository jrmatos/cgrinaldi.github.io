---
layout:     post
title:      Function Expressions vs. Declarations in JavaScript
date:       2015-03-26 00:00:00
summary:    An explanation of the key differences between function expressions and declarations.
tags: [javascript]
---

## Overview

In JavaScript there are two different ways of creating functions, and which way you use will have an impact on how you structure your program.

The first way is to use a **function declaration**, which looks like `function myFun (arg1, ...)`. The second way is to use a **function expression**, which looks like `var myFunc = function (arg1, ...)`. See the simple code blocks below demonstrating the two different formats in more detail:

<script src="https://gist.github.com/cgrinaldi/238f33685b64acd36ab5.js"></script>

## Key Differences

While the two formats look pretty similar, there are actually a number of key differences that you must account for when deciding which format to use.

### Function Expression

When using the function express form, you are clearly assigning an anonymous function to a variable, showing that functions are **first class objects** in JavaScript. At a fundamental level, this means that functions can be treated like any other object in JavaScript. This has the added benefit of letting you pass functions into other functions, or create functions that return other functions.

In addition, when using **function expressions**, JavaScript will only parse and create the associated object in memory if it gets to that part of the code.

### Function Declarations

**Function declarations** act quite differently from **function expressions**. The main difference is due to something called _hoisting_.

When the JavaScript interpreter begins running through your code, it does a first pass and will _hoist_ different parts of your code. Hoisting is the act of moving variable expressions up higher in the enclosing scope. The following code block shows how the interpreter looks at your code:

<script src="https://gist.github.com/cgrinaldi/929915b6ba656b4f2ea2.js"></script>

Where things get interesting is with **function declarations**. When functions are declared in this form, the entire function is actually hoisted to the top of its containing scope.

To make things clearer, let's see how the interpreter would look at the following code:

<script src="https://gist.github.com/cgrinaldi/0bc8166f9f0b71393382.js"></script>

While this code looks strange, and you might expect it to error out, you do actually get the desired output due to the hoisting of declared functions.

This simply does not work with **function expressions** because the interpreter hoists function expressions differently:

<script src="https://gist.github.com/cgrinaldi/c87971ea3411d8c3f89b.js"></script>

While the differences are not vast between **function expressions** and **function declarations**, hopefully this post helped to clarify what differences do exist.

