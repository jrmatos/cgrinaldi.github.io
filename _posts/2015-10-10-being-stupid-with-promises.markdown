---
layout:     post
title:      Being Stupid with Promises
date:       2015-10-10 00:00:00
summary:    A quick look at how I was using promises all wrong.
tags: [javascript, async, promises]
---

Lately I've been working on a project involving the use of Node.js as a backend. In particular, I've been reading and writing text files using the **fs** module. As you may know, such operations are asynchronous in nature, requiring the use of callbacks or promises.

I've really enjoyed using promises. They turn the following:

<script src="https://gist.github.com/cgrinaldi/84e26277c3d3f78ec139.js"></script>

into something much nicer:

<script src="https://gist.github.com/cgrinaldi/01acffbf056ca71e2f0f.js"></script>

In particular, I've been using the [Q Promise Library](https://github.com/kriskowal/q) (mostly because of my experience with Angular, and its $q service).

I thought I was a pro, using ```var deferred = Q.defer();```, ```deferred.resolve('someValue')```, and of course, ```return deferred.promise```. But then I read this great post about how [We have a problem with promises](http://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html). In particular, what caught my attention was "Rookie mistake #4: using 'deferred'".

**I had been using promises all wrong.**

Once I realized that the trick to using promises is to simply return other promises (rather than force the issue with ```deferred.resolve()```), I was able to turn this code:

<script src="https://gist.github.com/cgrinaldi/b64647aeec75e55a9b73.js"></script>

into this gorgeous piece of code:

<script src="https://gist.github.com/cgrinaldi/aa4c283d6fdac767eb25.js"></script>

I don't know about you, but there is something much more satisfying about that block of code than the one before it! ```nfbind``` does some magic that turns ```fs.readFile``` into a promise which I can then return. Feel free to read more about ```nfbind``` [here](https://github.com/kriskowal/q/wiki/API-Reference#qnfbindnodefunc-args).

I feel so much better now!


