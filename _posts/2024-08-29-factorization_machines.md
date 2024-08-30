---
title: "Factorization machines review"
date: 2024-08-29
categories: [Paper_review,EN]
tags: [factorization machines, d-way interaction, svm, recsys]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"

---

**Disclaimer**
This blog post is a summary of what I understood reading the paper. This is for personal use and might contain wrong information.
___


# Abstract

- FMs are a general predictor that can work with any real valued feature vector and can model interactions between variables with factorized parameters(comparative advantage to SVM)

- FMs' predicted value can be computed in O(n) time and FMs do not need support vectors in the solutions like SVM and parameters can reflect sparse settings compared to SVM.

- FMs can perform as well as other factorization methods that are purposely made for specific prediction tasks by specifying the input data.


# Introduction

SVMs can be applied to general tasks, but fail to learn parameters reliably under very sparse data, whereas specialized factorization models can learn reliably under sparse setting, but fail to be applicable to general tasks.
Factorization machine models interaction between variables by a factorized parameterization instead of a dense parameterization used in SVMs, leading to the the time complexity of O(n). 

In recsys, we are often encountered with problems where almost all of the elements$$ x_{i} $$of a vector 
