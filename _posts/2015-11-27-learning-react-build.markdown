---
layout:     post
title:      Learning React - Build Process
date:       2015-11-27 00:00:00
summary:    In this post, I briefly discuss setting up a build process using Gulp and Browserify.
tags: [react, gulp, browserify, webpack, learning, practice]
---

In React, you can write JavaScript code in a syntax called [JSX](https://facebook.github.io/react/docs/jsx-in-depth.html). JSX looks like HTML that is returned from your JavaScript functions, but, because this is not possible, you must first transpile your code into actual JavaScript.

The following code blocks show a before and after of the process of transpiling JSX into JavaScript. As you can see, JSX is much cleaner, and it better reflects the idea that React components render as DOM elements on the page.

<script src="https://gist.github.com/cgrinaldi/a677beb1f53c9bd65417.js"></script>

A couple of tips when working with JSX (and this is by no means complete):

- You cannot use `for` and `class` in JSX since they are reserved keywords in JavaScript. Instead, you must use `htmlFor` and `className`
- JSX is really a form of XML, and for this reason tags must be self closing (e.g., `<Label />` and `<Input />`)

To not drive yourself crazy, it helps to have the conversion of JSX to JavaScript integrated into a build process so you don't have to manually transpile the code at the command line. So for the past few days, I've been trying to get my build setup for React "just right". And looking back, I'm realizing **it was a complete, and utter waste of my time**.

I first got a [Gulp](https://github.com/LearningReact/skeleton-project/tree/browserify) solution (with Browserify and Reactify) up and running, but then, in the back of my head, I kept hearing a little voice say, "but [Webpack](https://webpack.github.io/) is now the 'standard' for React apps." So that led me to reading about and working through a number of tutorials on Webpack, React, and various ES2015 features.

I got Webpack up and running, but then the little voice was back with a vengeance, declaring that I would be much more efficient if I got [React hot loader](https://gaearon.github.io/react-hot-loader/) working, since then I could change a component and see everything on the screen update magically while maintaining the state of my components (seriously, see the link above, it is pretty sweet!). So that led to more research and experimentation, and to the detriment of my objective, more complexity. Until finally, I asked myself, why am I wasting my time doing this?

So I would say my first meta-learning lesson associated with learning React (and anything really) is the following:

<div class="boxed">Don't waste time optimizing and experimenting with something that is not integral to your learning objective.</div>

My goal through this series of blog posts is really to help me learn React and Flux, and setting up my build with Webpack vs. Gulp/Browserify/Reactify really isn't important in that context.

As an aside, I had a hard time finding a really simple React skeleton app, so I made my [own](https://github.com/LearningReact/skeleton-project/tree/webpack)! (with much help from [this post](http://jamesknelson.com/webpack-made-simple-build-es6-less-with-autorefresh-in-26-lines/) and [this one](http://jamesknelson.com/using-es6-in-the-browser-with-babel-6-and-webpack/)). The key is the `webpack.config.js` file:

<script src="https://gist.github.com/cgrinaldi/fbccc824d5516a4342b2.js"></script>

I also recreated a Gulp solution using help from [Tyler McGinnis' excellent series of React posts](http://tylermcginnis.com/reactjs-tutorial-a-comprehensive-guide-to-building-apps-with-react/).

Before concluding this post, I just want to make one point about frustration. If you are performing [deliberate practice](http://cgrinaldi.github.io/2015/11/22/learning-react-0/) correctly, then you will be pushing the limits of your current skill set. This is often a source of frustration (since it is always much nicer to just do something you are already good at!). That being said, I think frustration can also be a signal that little, unimportant things are preventing you from meeting your goals. It was only when I stepped back, and looked at the source of my frustration, that I realized that I was frustrated because I had yet to really learn anything related to React.

So with that, I'll conclude with one of my lessons learned box things:

<div class="boxed">If you are feeling frustrated while learning, take a step back and ask yourself why. Is it because what you are learning is hard? Or is it because some minor thing is preventing you from making progress...?</div>
