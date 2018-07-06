---
layout: post
title:  "First Order Logic"
date:   2016-02-18
categories: research
tags: first-order-logic
---
[First Order Loigc](http://mathworld.wolfram.com/First-OrderLogic.html), as known as first order predicate calculus, is defined by the following terms:

* [Term](http://mathworld.wolfram.com/Term.html), a variable, constant, or the result of a function. Denoted by upper case letters,e.g.,, A.

* [N-place function](http://mathworld.wolfram.com/Term.html), n is the arity of the function. n-place function means a function takes n arguments, and will return an object that has certain relations with those n inputs. Denoted by f.
* Predicate, an operator that will return either    or false. Predicates can be seen as a special type of functions which takes a set of arguments and return a    or false based on the definition of this predicate. Denoted by P.

* [Sentential formula](http://mathworld.wolfram.com/Predicate.html), an expression that represents a setence if we substitute variables to proper words (constants).

* [Atomic Statement](http://www.cogsci.rpi.edu/public_html/heuveb/teaching/Logic/CompLogic/Web/Handouts/FOL.pdf), constant, variable, 0-place function, and 0-place predicate.

* Universal quantifier, $\forall$, for all

* Existential quantifier, $\exists$, exists

* Scope of the respective quantifier, $\forall x B$, $B$ is the scope of the universal quantifier $\forall$.

* Free variable, a variable that is not in the scope of respective quantifier.

With above definitions, first-order predicate calculus is defined by the following rules:

1. Any atomic statement is a sentential formula.
2. If $B$ and $C$ are sentential formulas, then $\neg B$, $B \land C$, $B \lor C$, $B \implies C$ are all sentential formulas based on propositional calculus.
3. If $B$ ia a sentential formula in which $x$ is a free variable, then $\forall x B$, $\exists x B$ are sentential formulas.

