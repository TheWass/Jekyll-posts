---
layout: post
title: Introduction to Set Theory
date: 2015-01-22 15:10:24.000000000 -05:00
categories: []
tags:
- Mathematics
- set data structure
status: publish
type: post
published: true
meta:
  _wpcom_is_markdown: '1'
  _edit_last: '82057723'
  _publicize_pending: '1'
  geo_public: '0'
author:
  login: d35ruct0
  email: lord.d35ruct0@gmail.com
  display_name: The Wass
  first_name: ''
  last_name: ''
excerpt: !ruby/object:Hpricot::Doc
  options: {}
---
<p>This is intended to serve as a companion article to the formal models section of the Requirements Engineering series.  This document gives a short introduction to set theory and, in particular, the notations.  See also the Wikipedia page on <a href="http://en.wikipedia.org/wiki/Set_(mathematics)">Set (mathematics)</a>.<br />
<!--more--></p>
<hr />
<h1>Notation</h1>
<p>A set is a collection of distinct objects without regard to order.  The mathematical definition of a set matches well with the set data structure in computer science.  They are typically notated using curly brackets: <code>{4, 1, 3, 2}</code> is a set.  Duplicate values are implicitly discarded and order does not matter: <code>{4, 2, 1, 3} == {1, 2, 3, 4} == {1, 4, 3, 4, 2, 1}</code>  Ellipses indicate that the set continues in the obvious way: <code>{1, 2, 3, ..., 99, 100}</code> is the set of the first hundred positive integers.  Sets can also be notated by an expression, and assigned to a variable:  <code>F = {2n+1 : n is an integer; and 0 =&lt; n =&lt; 5}</code>  In this notation, the colon (:) means "such that".  This set can be interpreted as: F is the set of all numbers of the form 2n+1, such that n is a whole number in the range from 0 to 5 inclusive.  Also notice that lower case letters denote some element, whereas upper case letters indicate a set.  The empty set <code>{}</code> has a special notation: ∅.  The empty set is still considered a set, just with zero elements.</p>
<h1>Membership</h1>
<p>An element is said to be a member if it is contained within the set.  This is notated by &isin;. Consider: <code>a = 4</code> and <code>A = {4, 2, 3, 1}</code>. Therefore: <code>a ∈ A</code>.  This is read as "a is in A."  The 'fingers' of the symbol always point to the set: <code>A ∋ a</code> This is read as "A contains a."<br />
Consider two sets: <code>A = {4, 1, 2, 3}</code> and <code>B = {2, 1, 3, 4, 5}</code>.  If every member of set A is also a member of set B (as in this case), then A is said to be a subset of B.  This is written as <code>A ⊆ B</code>.  Alternatively, we can write <code>B ⊇ A</code>, read as B is a superset of A.  Notice the bar under each.  This indicates equivalency: if <code>B = {2, 1, 3, 4}</code> then <code>A ⊆ B</code> and <code>B ⊆ A</code>.</p>
<h1>Cardinality</h1>
<p>The cardinality of set is the number of members contained within that set.  For instance, the cardinality of <code>A = {4, 2, 3, 1}</code> is 4. Cardinality is notated as <code>|A|</code>.  In this case: <code>|A| = 4</code>.</p>
<h1>Special Sets</h1>
<ul>
<li>ℙ is the set of all primes: <code>{2, 3, 5, 7, 11, 13, 17, ...}</code></li>
<li>ℕ is the set of all natural numbers: <code>{0, 1, 2, 3, 4, 5, 6, ...}</code> *Sometimes 0 is omitted.</li>
<li>ℤ is the set of all integers: <code>{..., −2, −1, 0, 1, 2, ...}</code></li>
<li>ℚ is the set of all rational numbers: <code>{a/b : a, b ∈ Z, b ≠ 0}</code></li>
<li>ℝ is the set of all real numbers, rational and irrational numbers</li>
<li>ℂ is the set of all complex numbers: <code>{a + bi : a, b ∈ ℝ}</code></li>
</ul>
<h1>Operations</h1>
<p>These are some fundamental operations for constructing new sets from given sets.<br />
For all these examples, lets let <code>A={0, 1, 2, 3}</code> and <code>B={2, 3, 4, 5}</code></p>
<h2>Union</h2>
<p>Notation: <code>A ∪ B = {0, 1, 2, 3, 4, 5}</code><br />
The union operation combines the two sets together in one set.  The new set contains all elements from both parent sets.  This can be likened to the OR function in computer science.</p>
<h2>Intersection</h2>
<p>Notation: <code>A ∩ B = {2, 3}</code><br />
The intersection operation finds elements in both sets that are equivalent and creates a new set.  The new set contains only elements found in both parent sets.  This can be likened to the AND function in computer science.</p>
<h2>Compliment</h2>
<p>Notation: <code>A' = {5, 6, 7, 8, ...}</code><br />
The compliment operation involves one set, and includes everything <em>not</em> in that set.  This usually must be identified with some domain, or some universal set.</p>
<h2>Subtraction</h2>
<p>Notation: <code>A - B = {0, 1}</code><br />
The subtraction operation takes both sets and excludes all elements in one set, leaving unique elements in the other. A - B includes all elements in set A that do not exist in set B. Also notice: <code>B - A = {4, 5}</code></p>
<h2>Cross Product</h2>
<p>Notation: <code>A x B = {(0,2), (0,3), (0,4), (0,5), (1,2), (1,3), (1,4), (1,5), (2,2), (2,3), (2,4), (2,5), (3,2), (3,3), (3,4), (3,5)}</code><br />
The cross product combines both sets in a non-destructive manner.  It associates every element in one set with every other element in the other set.  This is also called the Cartesian product.</p>
<h1>Predicate Logic</h1>
<p>I'll end with a little bit of predicate logic. Specifically, a mathematical method of 'iterating' over a set.  There are two notations:<br />
∀ means for all or for each, and ∃ means there exists at least one. So, <code>∀x: P(x)</code> means that P(x) is true for all x.  <code>∃x: P(x)</code> means there is at least one x such that P(x) is true.<br />
<code>∀ n ∈ ℕ: 2n ≥ n</code> translates to: "For every n in the set of natural numbers, 2n is greater or equal to n."<br />
<code>∃ n ∈ ℕ: n/2 ∈ ℕ</code> translates to: "There exists an n in the set of natural numbers, where n/2 is also in the set of natural numbers (n is even)."</p>
<hr />
