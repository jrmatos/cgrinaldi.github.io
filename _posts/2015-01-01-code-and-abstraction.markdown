---
layout:     post
title:      Code Abstraction and Functional Programming
date:       2015-01-01 14:00:00
summary:    The one where I marvel at code abstraction and functional programming, and talk about a code pyramid.
tags: [functional-programming, programming, javascript]
---

As part of the Hack Reactor Precourse curriculum, we were asked to implement a variant of the JavaScript library [Underscore](http://underscorejs.org/). The basic idea behind this library is to add functional programming methods to help with iterating, and acting on, collections of values. These functions include `each`, `map`, `filter`, and many others.

I've only had minimal exposure to functional programming in Python, but this project really helped me to explore it in further detail. What I found fascinating about implementing these particular methods is the idea of abstracting away certain details. Instead of building each function from scratch, we were encouraged to use previously constructed functions to build up the solution.

It is effectively like building tools to help build other tools, and due to the complexity of the code involved, it almost necessary to do it in this manner. The human mind (at least mine!) is unable to hold all of the details of all of the functions at one time. Instead, it is imperative to understand the _ideas_ that are being used, and ignore the nitty gritty code that is actually doing all of the work.

Using a different metaphor, I imagine that looking at code is almost like looking at a pyramid. At the top of this **code pyramid** is the one thing the code is supposed to achieve. For example, in the case of a website it might be to sell products to customers. However, to actually achieve this purpose, we will have to create first-order supporting functions (for example, something that adds shopping cart functionality to the site). As we descend down our code pyramid, we see that the first-order functions need the support of second-order functions to achieve their sub-purpose. And so on.

I have found it useful to move up or down the code pyramid depending on what level of abstraction is most useful for my current task. Sometimes this will be at the base of the pyramid, dealing with a fairly low-level function. Other times, I might be assembling the last few functions needed to accomplish the overarching purpose. The point, though, is that the entirety of the code pyramid doesn't need to be held in your mind. Just a particular segment.

To help make the above discussion a little more concrete, let's actually look at some code and see how abstraction can help us. In this particular case, we are going to take the simple act of iterating over an array and printing its values to the console:

{% highlight javascript %}
  // Iterating over an array

  var anArray = [1,2,3,4,5];
  for (var i = 0; i < anArray.length; i++) {
    console.log(anArray[i]); // prints 1,2,3,4,5 to console
  }

{% endhighlight %}

Every time we iterate over an array in this manner, we have to remember the order of the parameters in the `for` loop, make sure we have a counter variable that is correctly incremented, and of course, we can't forget the necessary brackets and parentheses! This all seems quite unnecessary if you think about what we are actually trying to do, which is simply iterate over an array.

The beauty of abstraction is that we are going to code the necessary mechanics needed to iterate over an array once, and then use that abstracted function whenever we need to perform array iteration. The benefit of this is twofold:

1. We only have the code needed to implement iteration in one place (following the **Don't Repeat Yourself (DRY) Principle**); and
2. It allows the rest of our code to be much more understandable and streamlined. So let's begin!

We know our new function should take in the array that we want to iterate over. So as a first pass, we may produce something like the following:

{% highlight javascript %}
  // Creating a function to print array values

  var forEachValPrint = function(array) {
    for (var i = 0; i < array.length; i++) {
      console.log(array[i]);
    }
  }

  forEachValPrint([1,2,3]) // prints 1, 2, 3 to the console
  forEachValPrint(["cat", "dog"]) // prints 'cat', 'dog'

{% endhighlight %}

So now, whenever we want to print an array's contents to the console, we can simply use `forEachValPrint` instead of our earlier looping method. This is less code, but more importantly, it codifies the _idea_ of what we are trying to do.

However, you may notice that `forEachValPrint` seems overly specialized, and in fact, we can do much better. Instead of abstracting the for loop to only working with printing values to the console (and seriously, how useful is that?), let's abstract this function further.

What we are really trying to do is simply get across the idea of doing _something_ with each individual value of an array. So in this case, the parameters to our function will be the array (as before), but we will also pass in the action that we want to do to each value. This is easy to do in JavaScript because functions are treated like any other values (numbers, booleans, etc.), which means they can be used as parameters in functions, and even returned from them. In code, we can do the following:

{% highlight javascript %}
  // Creating a function that abstracts array iteration

  var forEach = function(array, action) {  // action is a function
    for (var i = 0; i < array.length; i++) {
      action(array[i]);
    }
  }

  forEach([1,2,3], console.log) // prints 1, 2, and 3 to the console

{% endhighlight %}

So here we see that we are passing in the function `console.log` to our `forEach` function. `forEach` is then iterating over the array, passing each value it encounters to `console.log`, which simply prints the value out. This doesn't seem too interesting because that is exactly what we were doing before. However, watch what we can do with our new function:

{% highlight javascript %}
  // Different ways of using forEach()

  var doubled = [];

  var doubleVal = function(val) {
    doubled.push(2 * val);
  };

  forEach([1,2,3], doubleVal); // passing doubleVal() to forEach()
  forEach(doubled, console.log) // prints 2, 4, 6

{% endhighlight %}

So a couple of things are going on here. First, we created a new function called `doubleVal` that will push a value multiplied by two to the `doubled` array. Because we are using `doubleVal` in `forEach`, it is doing this to each and every value in the array. Then, in the very next line, we use `forEach` to once again print the contents of the `doubled` array. This shows that because we have abstracted out the idea of iteration, we can accomplish more with less code, while still being able to understand at a high-level what our code is trying to achieve.

If we were to continue down the path of abstraction (and code the necessary second-order functions), we could create something like the following:

{% highlight javascript %}

console.log(sum(range(1, 10))); // print the sum of numbers 1 to 10

{% endhighlight %}

The above code block reads very naturally, and it is easy to see what we are trying to achieve. This is the power of abstraction.

I've only touched the surface here, but for more detail, I highly recommend [Chapter 5 of Eloquent JavaScript](http://eloquentjavascript.net/05_higher_order.html) (the whole book is great too!).



