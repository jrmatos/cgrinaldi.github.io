---
layout:     post
title:      Testing DOM JavaScript Functions with Mocha
date:       2015-02-09 00:00:00
summary:    The one where I explain how I figured out how to test JavaScript functions that depend on the DOM using Mocha.
tags: [javascript, mocha, tdd]
---

During the first week of Hack Reactor, we were exposed to many different ideas. The one I found most intriguing was this idea of Test Driven Development (TDD). In TDD, you write tests that check the functionality you are trying to implement, write code to make those tests pass, and finally, refactor your code.

I really liked the idea of TDD since it is almost a game. In the first half of the game, you are competing with future you, trying to create tests that accurately reflect what functionality you actually want to implement. In the second half of the game, you are working to make a bunch of nasty red Xs turn green. When they do, you know you have correctly implemented the functionality that you intended (assuming that past you did a good job creating the tests in the first place!).

In this post, I want to detail how I solved a particular problem that I ran into while attempting to test a function that operated on the DOM.

As a quick overview, the goal of the function was to take a DOM node as input, and output whether or not that node contained five or more `div` nodes (including itself).

Following the TDD approach, I initially wrote the following tests (using the [Mocha Test Framework](http://mochajs.org/)) and the [Chai Assertion Library](http://chaijs.com/):

{% highlight javascript %}
describe('recursion-divs', function() {
  var html = '<div></div><div></div><div class="addHere"></div>';
  $('body').append(html);

  it('should return false when there are fewer than 5 divs', function() {
    var result = containsFiveOrMoreDivs(document.body);
    expect(result).to.equal(false);
  });

  $('body').append(html);

  it('should return true when there are more than 5 divs', function() {
    expect(containsFiveOrMoreDivs(document.body)).to.equal(true);
  });
});
{% endhighlight %}

My initial thought was to start with a DOM that had less than 5 div elements, test that my function returns false, and than add enough div elements to test the affirmative case.

However, when I created my recursive function and ran the tests, I was greeted with the following horror:

<div style="text-align: center;">
  <img src="../../../../images/2015-02-09-failed-tests.png" style="width: 500px;">
</div>

Not good, not good at all! Turns out my initial intuition was wrong. The order of operations I thought was happening did not in fact happen. Instead, the test suite runs all of the code before outputting the results of my tests, causing both DOM operations (`$('body').append(html)`) to run and create a DOM tree that has more than 5 divs (the reason why I pass only the second test).

I realized the issue was that my initial hypothesis of the test order was wrong, causing my `expect()`s to both be looking at the same DOM tree.

To fix this, I simply used the jQuery `clone()` method to create a second DOM tree for which I could test independently of the first one. Here is the resulting code:

{% highlight javascript %}
var expect = chai.expect;

describe('recursion-divs', function() {
  // create html string that will be inserted into DOM
  var html = '<div></div><div></div><div></div>';

  // append html to body node
  $(document.body).append(html);

  // run my original test
  it('should return false when there are fewer than 5 divs', function() {
    var result = containsFiveOrMoreDivs(document.body);
    expect(result).to.equal(false);
  });

  // create a clone of the DOM, and append more divs to it
  var $domClone = $(document.body).clone();
  $domClone.append(html);

  // test this new DOM
  it('should return true when there are more than 5 divs', function() {
    // jQuery returns an array of DOM nodes, so use 0 index
    expect(containsFiveOrMoreDivs($domClone[0])).to.equal(true);
  });
});
{% endhighlight %}

And last but not least, the passing tests!

<div style="text-align: center;">
  <img src="../../../../images/2015-02-09-passing-tests.png" style="width: 400px;">
</div>



