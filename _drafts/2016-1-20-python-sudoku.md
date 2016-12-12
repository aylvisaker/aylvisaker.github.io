---
layout: post
title: "Python Sudoku"
categories: [projects]
tags: [puzzles, permutations, code]
description: Sudoku Generator
---

# Python Sudoku
[Github Repository](https://aylvisaker.github.io/python-sudoku)

### Required Packages
For speed, we'll make use of the packages `numpy` and `gmpy2` (`gmpy` would work as well).

### Data Structure Choices and Setup
Solving a sudoku puzzle is a process that can be implemented in many different ways. We will be implementing both a brute force search (which is very fast), and a logic-based approach (which will allow us to rate puzzle difficulty). A standard sudoku puzzle is completed on a $9 \times 9 $ grid, but it is quite easy to generalize the same strategies to an $n\times n$ grid, so we shall do just that.


### Future Work
* Check a puzzle for multiple solutions.
* Generate a puzzle.
* Solve and grade a puzzle using only logic.
* Multi-sudoku puzzles and other variants.
