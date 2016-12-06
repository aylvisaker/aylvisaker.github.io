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

### Prerquisites, constants, and tools
We will need some packages. But not all of these.
{% highlight python %}
import os, sys
basis = 3
squares = list(range(basis ** 4))
blocks = squares[i*basis:(i+1)*basis for i in basis]
def cross(a, b):
    return []
{% endhighlight %}

{% highlight python %}
{% includes sudoku.py %}
{% endhighlight %}

Here is some `inline code`. Next is a table

|the     |quick   |brown  |
| :----- | -----: | :---: |
|fox     |jumps   |over   |
|the     |lazy    |dog.   |
