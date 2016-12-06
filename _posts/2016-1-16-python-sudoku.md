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
- [X] Solve sudoku puzzles.
- [X] Solve sudoku puzzles with arbitrary basis.
- [ ] Check a puzzle for multiple solutions.
- [ ] Generate a puzzle.
- [ ] Solve and grade a puzzle using only logic.

### Prerquisites
We will need some packages 
```python
import os, sys, time, math, numpy
```

### Constants and tools
```python
basis = 3
squares = list(Range(basis ** 4))
blocks = squares[i*basis:(i+1)*basis for i in basis]
```
