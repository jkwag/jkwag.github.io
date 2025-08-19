---
title: "FRM Module 12 핵심 정리"
date: 2025-08-19
categories: [FRM]
tags: []
toc: true
math: true
toc_label: "Table of contents"
toc_icon: "cog"
---

## Event
- An event is one of the possible outcomes or a subset of the possible outcomes of a random event.
- **Event space** is all the **subsets of possible outcomes** and the **empty set**

## Independence
- Two events are independent if either of the following hold:
  - $P(A) \times P(B) = P(AB)$
  - $P(A|B) = P(A)$
- Two events are mutually exclusive if the joint probability $P(AB) = 0$ -> $P(A or B) = P(A) + P(B)$

## Conditional independence
- Two events are conditional on a thrid event are independent -> conditioanlly independent
- $P(AB|C) = P(A|C)P(B|C)$, then A and B are conditionally independent
- Two events may be **independent but conditionally dependent**, or vice versa

## Probability function
- A probability function describes the probability for each possible outcome for a discrete probability distribution

## Joint probability
- The joint probability of two events is the probability that they will both occur
  - $P(AB) = P(A|B) \times P(B)$
  - $P(A|B) = \frac{P(AB)}{P(B)}$

## Unconditional probability
- probability of an event occurring
- A **conditional probability** is the probability of an Event A occurring **given event B has occurred**

## Bayes'rule
- $P(A|B) = \frac{P(AB)}{P(B)}$
  - This formula allows us to update the unconditional probability, P(A), based on the fact that B has occurred
  - P(AB) can be calculated as P(B|A)P(A) 
