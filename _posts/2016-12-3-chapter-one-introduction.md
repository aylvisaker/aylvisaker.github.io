---
layout: post
title: "Chapter 1 Notes"
categories: [notes]
tags: [notes]
description: Course prerequisites.
---

# Introduction
This class assumes basic familiarity with **arithmetic**, **algebra**, and **sets** (particularly the fundamental theorems of the former).

### Important sets

* The **additive identity** is \\(0\\). The **multiplicative identity** is \\(1\\). The set of **natural numbers** is \\(\\mathbb{N}\\), the smallest set that contains \\(\\{0,1\\}\\) and is closed under addition, multiplication, and exponentiation.
\\[ \mathbb{N} = \\{0,1,2,\dots\\} \\]

* The **additive inverse** of a number, \\(n\\), is the number \\(-n\\). The set of  **integers** is  \\(\\mathbb{Z}\\), the smallest set that contains \\(\\mathbb{N}\\) and is closed under additive inverses, addition, subtraction, and multiplication. \\(\\mathbb{Z}\\) is not closed under exponentiation.
\\[ \mathbb{Z} = \\{\dots,-2,-1,0,1,2,\dots\\} \\]

* The **multiplicative inverse** of a non-zero number, \\(n\\), is the number \\(\\frac{1}{n}\\). The set of **rational numbers** is  \\(\\mathbb{Q}\\), the smallest set that contains \\(\\mathbb{Z}\\) and is closed under inverses, addition, subtraction, multiplication, and non-zero division. \\(\\mathbb{Q}\\) is not closed under exponentiation.
\\[ \mathbb{Q} = \\left\\{\\frac{m}{n} \mid m,n\in\mathbb{Z},n\neq0\\right\\} \\]

### Gaps in the rational numbers

**_Theorem_**. \\(\\sqrt{2}\notin\\mathbb{Q}\\).

**_Proof_**. Suppose, by way of contradiction, that \\(\\sqrt{2} = x = \frac{a}{b}\\) where \\(a,b\\in\\mathbb{Z}\\) and \\(b\\neq0\\). Then let \\(y = x^2\\cdot b = 2b^2= a^2)\\). Observe that \\(a, b\\), and \\(y\\) are integers, and so by the fundamental theorem of arithmetic, have unique prime factorizations.

\\[a = \\prod\_{k=1}^{r}p\_{i\_k}^{d\_k}\\]
\\[b = \\prod\_{k=1}^{s}q\_{i\_k}^{e\_k}\\]

Now consider the prime factorization(s) of \\(y\\):

\\[y = a^2 = \\prod\_{k=1}^{r}p\_{i\_k}^{2d\_k}\\]
\\[y = 2b^2 = 2\\prod\_{k=1}^{s}q\_{i\_k}^{2e\_k}\\]

These cannot be the same because the former factorization has an even exponent on the prime \\(2\\), while the latter has an odd exponent on the prime \\(2\\). This contradicts the uniqueness part of the fundamental theorem of arithmetic for the integer \\(y\\). Our supposition that \\(\\sqrt{2}\\in\\mathbb{Q}\\) must have been incorrect. \\(\\Box\\)

### Set notation and basic definitions

* \\(x\\in S\\) means that \\(x\\) is an **element** (i.e. **member**) of \\(S\\) and \\(x\\notin S\\) means that \\(x\\) is *not* an element of \\(S\\).
* \\(\\emptyset = \\{\\}\\) is called the **empty set** and has no elements. Sets which have at least one element are called **non-empty**.
* If \\(A\\) and \\(B\\) are sets, then 
  * \\(A\\cap B = \\{x\\mid x\\in A\\text{ and } x\\in B\\}\\) is the **intersection** of \\(A\\) and \\(B\\).
  * \\(A\\cup B = \\{x\\mid x\\in A\\text{ or }  x\\in B\\}\\) is the **union** of \\(A\\) and \\(B\\). 
  * \\(A\\subset B\\) and \\(B\\supset A\\) both convey that each element of \\(A\\) is also an element of \\(B\\). We say that \\(A\\) is **contained** in  (i.e. a **subset** of) \\(B\\).
  * \\(A=B\\) means that \\(A\\subset B\\) and \\(B\\subset A\\).

# Ordered Sets
