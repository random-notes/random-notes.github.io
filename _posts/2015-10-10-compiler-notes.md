---
layout: post
title:  "Compilers Notes"
date:   2015-10-10
categories: book-notes
tags: compiler
---

# First and Follow set

First set can be computed by the following rules:


1. If there are terminals at the start of a production rule, add them into first set. A -> a B, where a is a terminal.
2. If there are nonterminals at the start of a production rule, add their first set into first set. A -> B, where B is a nonterminal.
3. If one ore more nonterminals can be dreived to epsilon, find the first thing after all the epsilons, apply 1 and 2.

Follow set can be computed by the following rules:


1. Add $ into follow set
2. Find all occurrence of the non-terminal of interest that appears on the right hand side.
3. If it is the last token in a production rule, for example A -> xxxxxB, then B contains all the follows of A.
4. If it is followed by other tokens, for example A -> xxxBC, if C is terminal, put C into B‘s follow set, if non-terminal, put first(C) into follow(B).
5. If things following B can dreive to epsilon, find the first thing after all epsilons, apply 2 and 3.

# LL(1) Grammar


* No left recursion: E -> E + T is invalid.
* No common left prefix: E -> ab | ac is invalid
* No ambiguity: S -> if E then S | if E then S else S (dangling else)

## How to solve left recursion

* E -> E + id
* E -> id
* E -> (E)

First, find the production rule with left recursion E -> E + id, then cut off the right part after the recursive non-terminal E, which is +id, create a new production rule E' -> +id E'|e. Convert the original left recursive rule to E -> idE'.

## How to solve common prefix

For production rule E -> ab | ac, extract common prefix, which in this case is a and construct two new production rule E -> aE' and E' -> b | c.

Note that **left factoring can not solve ambiguity**:

S -> if E then S and S -> if E then S else S can be converted into S -> if E then S S', S' -> else S | e. But in certain cases we can not determine which if an else belongs to (the dangling else problem).

## LL(1) parse table

After we get the first and follow sets, we do the following

1. For First(A), write corresponding production rule at [A,x] where x is an item in First(A) except epsilon.
2. For epsilon, write all corresponding production rule at [A,y], where y is an item in Follow(A).

# LR(0) Grammar

If there are shift and reduce in the same configuration set, then it is not a LR(0) grammar. If we consider the SLR parsing table, it is a LR(0) grammar if there is no shift and reduce on the same row.

# SLR(1) Grammar

It is a SLR1 if in a SLR parsing table, there is no shift and reduce in the same cell. We reduce when the following character is in Follow(A) where A is the current nonterminal on the top of the stack.

An example that is not SLR is


* S -> L = R
* S –> R
* L –> *R
* L –> id
* R –> L

because when

* S -> L. = R
* R -> L.

Follow(L) contains =, therefore we can not distinguish shift and reduce by looking at next symbol =.

# LR(1) Grammar

Instead of reduce by items in Follow, we use a lookahead Here


* S' -> .S, $
* S -> .L = R, $
* S -> .R, $
* L -> .*R, =
* L -> .id, =
* R -> L, $

Note we write down the lookahead of an item, for example, S' -> .S ends with $, and S -> .L = R. Therefore the token follows S -> .L = R, $ is $. And for S -> .L = R, $, we also have L -> .*R, and because L is followed by = in the previous item, so L -> .*R, =.

A grammar is not LR(1) only if there is a shift-reduce conflict that has an overlapped lookhead set.

# LALR(1) Grammar

This is based on LR(1) automaton but merge items with same core (but different lookheads).

If we have two items

* S -> R, a/b
* S -> R, $

then we combine them into one item because they have the same core S -> R

* S -> R, $/a/b

# Reference:
[LL1 Parser](http://research.microsoft.com/en-us/um/people/abegel/cs164/ll1.html)
[SLR and LR Parsing](https://web.stanford.edu/class/archive/cs/cs143/cs143.1128/handouts/110%20LR%20and%20SLR%20Parsing.pdf)
