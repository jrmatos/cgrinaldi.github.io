---
layout:     post
title:      Automating Git Pull using Bash Scripting
date:       2015-02-24 00:00:00
summary:    A quick tutorial on automating the process of performing git pull on a collection of local repositories.
tags: [automation, bash, git, github]
---

## Overview

At Hack Reactor, we rely on Github to manage the many project sprints that we are exposed to on a daily basis. The day we complete a a sprint (every two days!), Hack Reactor gives us access to a solution branch that we can pull into our local repository.

Because of the rapid pace of the program, I have found myself often forgetting to pull solutions down. And because we have to do this on a daily basis, I have developed a huge backlog of out-of-date repos that need updating. I figured automating this process would be a good way get rid of that backlog. Plus, it would also give me the opportunity to learn some Bash scripting!

## Code

I assume that we have a central directory that contains multiple directories that each contain a git repository. As you will see in the code below, this assumption allows us to iterate through each of the repo-containing directories and run the necessary git commands.

### Check Statuses

The first code block demonstrates the overall structure of how I approached the problem of scripting git commands. It simply loops through each directory and calls `git status` after checking out the _master_ branch. I wanted to implement this first in order to quickly look at the state of all of my repos.

Since we are going to be hitting Github in rapid succession, it makes sense to cache our login information. See this [link](https://help.github.com/articles/caching-your-github-password-in-git/) on how to do so.

<script src="https://gist.github.com/cgrinaldi/abb812f4e23a4ce97a87.js"></script>

The key to making this script work is the `git -C` commmand. It allows us to run git commands in a different directory than the one we are currently in. That being said, make sure you actually intend to run the script's git commands before doing so!

### Pull Solution Branch

This next code block is the one that I created in order to automatically merge in remote branches (some of which are updated on a daily basis). I make the following assumptions:

- Each remote repository subscribes to a consistent naming convention for the URL (in our case, [https://github.com/hackreactor/REPO-NAME-HERE.git](https://github.com/hackreactor)).
- Each remote repository has the same name for the branch that you wish to automatically merge in (in our case, _solution_).

<script src="https://gist.github.com/cgrinaldi/e1d5464b823659fdcd75.js"></script>

I recommend placing the above code blocks into bash profile functions. Then simply navigate to the directory that contains the repository-containing directories and run the functions.

As a further optimization (mainly applicable to Hack Reactor students), I recommend changing `for d in */; do` to `for d in [TOY DIRECTORY]/; do` if you just want to pull in the latest toy problem solution from the previous day. This will prevent the script from hitting a bunch of repos that do not need updating.

HR students can use code similar to the blocks above to automate the process of pulling down the latest toy problem. In the future, I'll probably implement something along these lines using a chron job. Yay laziness! :)

If you have any questions/comments/corrections, please feel free to email me or leave them in the comments below. Thanks for reading!