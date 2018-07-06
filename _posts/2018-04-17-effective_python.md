---
layout: post
title:  "Effective Python: 59 Specific Ways to Write Better Python"
date:   2018-04-17
categories: coding
tags: python
---

> Work in progress

# PEP8 style guide

* Separate functions and classes with two blank lines; Separate methods with one blank line.
* Continuations of long expression onto next line should add an extra indentation.
* Check empty values by checking `if not SOME_VAR`.
* Import order: standard libraries, third-party libraries, and own modules.

* Name function/variable/attributes with `lowercase_underscore`.
* Name protected instance attributes with `_leading_underscore`.
* Name private instance attributes with `__double_leading_underscore`.
* Name classes and exceptions with `CapitalizedWord`.
* Name module-level constants with `ALL_CAPS`.
* Name first parameter in instance methods and class methods as `self` and `cls` respectively.

