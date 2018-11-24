---
title: Solving Monty Hall Problem using Bayes Theorem
layout: post
date: 2018-11-24 10:49:51 -0800
categories:
- Book Notes
tags:
- Think Bayes
- Bayesian
- Book Notes

---
## Problem Definition

The [Monty Hall problem - Wikipedia](https://en.wikipedia.org/wiki/Monty_Hall_problem) is a famous counter-intuitive statistic puzzle. The definition is as follow:

> Suppose you're on a game show, and you're given the choice of three doors: Behind one door is a car; behind the others, goats. You pick a door, say No. 1, and the host, who knows what's behind the doors, opens another door, say No. 3, which has a goat. He then says to you, "Do you want to pick door No. 2?" Is it to your advantage to switch your choice? 

The basic assumptions of this problem are:

1.  The host must always open a door that was not picked by the contestant ([Mueser and Granberg 1999](https://en.wikipedia.org/wiki/Monty_Hall_problem#refMueserandGranberg1999)).
2.  The host must always open a door to reveal a goat and never the car.
3.  The host must always offer the chance to switch between the originally chosen door and the remaining closed door.

## Revisit Bayes Theorem

Now before we talk about the Monty Hall problem, please allow me to revisit the Bayes Theorem. Suppose we have two events $A$ and $B$, then the probability of both $A$ and $B$ happen would be $P(A,B)$. If $A$ and $B$ are independent, then we can rewrite the probability to

$$P(A,B) = P(A)P(B),$$

However, it is not always the case that $A$ anb $B$ are independent, therefore we need to rewrite the probability to

$$P(A,B) = P(A)P(B|A),$$

and 

$$P(B,A) = P(B)P(A|B).$$

Because conjunction is commutative, i.e. $P(A,B) = P(B,A)$, therefore

$$P(A)P(B|A) = P(B)|P(A|B).$$

So, finally, we can get $P(B | A)=\frac{P(B) | P(A | B)}{P(A)}$. If we use the [diachronic interpretation](https://u.osu.edu/jeon.96/personal-projects/math/bayesian-statistics-diachronic-interpretation/), then we rewrite $B$ to $H$, which denotes the _hypothesis_, and rewrite $A$ to $D$ or $E$, which denotes for either _data_ or _evidence_. Here I'll stick with $D$ because that is what [Think Bayes](https://www.amazon.com/Think-Bayes-Bayesian-Statistics-Python/dp/1449370780) uses.

Okay, now the equation becomes

$$P(H|D) = \frac{P(H)P(D|H)}{P(D)}.$$

We can read this as the probability of hypothesis given data ($P(H|D)$, **posterior**) equals to the probability of hypothesis ($P(H)$, **piror**) multiply the probability of data given hypothesis ($P(D|H)$, **likelihood**), divided by the probability of data ($P(D)$, **normalizing constant**).

## Apply Bayes Theorem to Monty Hall Problem

Now let's apply Bayes Theorem to Monty Hall problem. First, we define the three doors as $a$, $b$, and $c$. Say I choose door $a$, and Monty Hall opens door $b$, which does not have a car behind it, and asking me whether or not to switch my choice to door $c$.

Following the [Think Bayes](https://www.amazon.com/Think-Bayes-Bayesian-Statistics-Python/dp/1449370780) book, I'll first define a table to compute **piror**, **likelihood**, and **normalizing constant**.

| Hypothesis $H$             | Piror $P(H_i)$ | Likelihood $P(D\|H_i)$ |
| -------------------------- |:--------------:|:----------------------:|
| $H_1$: Car behind door $a$ | $\frac{1}{3}$  |     $\frac{1}{2}$      |
| $H_2$: Car behind door $b$ | $\frac{1}{3}$  |          $0$           |
| $H_3$: Car behind door $c$ | $\frac{1}{3}$  |          $1$           |
_Note that is this problem, the **data** is Monty Hall opens door $b$ and there is no car behinds it._

Now I'll explain how we get those values. First, all pirors are $\frac{1}{3}$ because without any extra information from the data, each door has the same probability to have that car behinds it.

Hypothesis $H_1$ is that the car is behind door $a$, which is the door we choose. In that case, Monty Hall can open door $b$ or $c$ because none of them has a car behinds it. Therefore, $P(D|H_1)$ equals to $\frac{1}{2}$, because the probability of Monty opens door $b$ is $\frac{1}{2}$ now.

Hypothesis $H_2$ is that the car is behind door $b$. In that case, Monty Hall can not open door $b$ and show there is no car behind $b$. Therefore, the likelihood $P(D|H_2)$ should be zero because the _data_ simply can not exist given the hypothesis.

Hypothesis $H_3$ is that the car is behind door $c$. Under that hypothesis, Monty has to open door $b$ because he must always open a door without a car. So, the likelihood $P(D|H_3)$ is $1$.

Summing up, $P(H_1|D)$ becomes

$$P(H_1|D) = \frac{P(H_1)P(D|H_1)}{D} = \frac{P(H_1)P(D|H_1)}{\sum_{i=1}^{3}P(H_i)P(D|H_i)} = \frac{\frac{1}{3}\times\frac{1}{2}}{\frac{1}{3}\times\frac{1}{2} + \frac{1}{3}\times0 + \frac{1}{3}\times1} = \frac{1}{3}.$$