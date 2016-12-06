---
layout: post
title: "Python Sudoku"
categories: [projects]
tags: [puzzles]
description: Sudoku Generator
---

[Github Repository](https://aylvisaker.github.io/python-sudoku)

# Python Sudoku
### Goals
* Solve sudoku puzzles.
* Solve sudoku puzzles with arbitrary basis.
* Check a puzzle for multiple solutions.
* Generate a puzzle.
* Solve and grade a puzzle using only logic.

### Prerquisites
We will need some packages. But not all of these.
{% highlight python %}
import os, sys, time, math, numpy
{% endhighlight %}

### Constants and tools
{% highlight python %}
basis = 3
squares = list(range(basis ** 4))
blocks = squares[i*basis:(i+1)*basis for i in basis]
{% endhighlight %}

Here is some `inline code`. See if it works. The table is a test as well.

\|the     \|quick   \|brown  \|
\|--------\|--------\|-------\|
\|fox     \|jumps   \|over   \|
\|the     \|lazy    \|dog.   \|
