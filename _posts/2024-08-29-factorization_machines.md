---
title: "Factorization machines review"
date: 2024-08-29
categories: [Paper_review,EN]
tags: [factorization machines, d-way interaction, svm, recsys]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
math: true
---

**Disclaimer**
This blog post is a summary of what I understood reading the paper. This is for personal use and might contain wrong information.

---


# Abstract

- FMs are a general predictor that can work with any real valued feature vector and can model interactions between variables with factorized parameters(comparative advantage to SVM)

- FMs' predicted value can be computed in O(n) time and FMs do not need support vectors in the solutions like SVM and parameters can reflect sparse settings compared to SVM.

- FMs can perform as well as other factorization methods that are purposely made for specific prediction tasks by specifying the input data.


# Introduction

SVMs can be applied to general tasks, but fail to learn parameters reliably under very sparse data, whereas specialized factorization models can learn reliably under sparse setting, but fail to be applicable to general tasks.
Factorization machine models interaction between variables by a factorized parameterization instead of a dense parameterization used in SVMs, leading to the the time complexity of O(n). 

In recsys, we are often encountered with problems where almost all of the elements $x_i$ of a vector are zero. This problem arises from the large scale of categorical variable domains. For example, in the case of movie recommendation tasks, the user-item matrix consists of each row representing users, and each column representing movies and intuitively, even though it is likely that not many users have rated more than 10 movies in the dataset, data for movies each user has not seen still goes in with 0.

<img width="654" alt="Screenshot 2024-09-06 at 10 37 11 AM" src="https://github.com/user-attachments/assets/dcf01cb9-426b-4dae-a96b-b583ebbb9268">

The image above looks quite a lot different from the user item matrix presented in matrix factorization. The form seemingly resembles matrix form often presented in multiple variable regression. One thing to note is that not only the matrix includes users and movies as its own features, it also has features like Other movies rated, time and etc.

# Factorization machines

## Model equation 

The model equation for a fractorization machine of degree d = 2 is as follows:

$$\hat{y} = w_{0} + \sum_{i=1}^{n}w_{i}x{i} + \sum_{i=1}^{n}\sum_{j= i+1}^{n}<v_{i},v_{j}>x_{i}x{j}$$

The equation is strikingly similar to polynomial regression if we are to consider the $w_{0}$ term as bias and $w_i$ coefficients for each variable. The interaction coefficient $<v_{i}, v_{j}>$ is what makes this model special. i and j here respectively denote i th and j th variable out of n variables. And, $V\in R^{n\times k}$, meaning each variable is mapped to a vector of dimension $k$. So, continuing with the example image above, for example, User A variable will be mapped to a vector of dimension k 

$$\begin{bmatrix}
A_{1} & A_{2} & ... & ... & A_{k-1} &A_{k}\\
\end{bmatrix}$$ 

and Movie TI(Titanic) will also be mapped to a vector of dimension k, which will also result in 

$$\begin{bmatrix}
Ti_{1} & Ti_{2} & ... & ... & Ti_{k-1} & Ti_{k}\\
\end{bmatrix}$$ 

where $k \in N_{0}^{+}$, which we ought to determine as a hyperparameter. In the paper, authors argue that restricting k will lead to better generalization and improved interaction matrices under sparsity.

## Parameter estimation under sparsity

Since the matrix is sparse, one might ask a question of how are we supposed to estimate interactions when each data point is likely to have most of its entries 0? Let us say we want to estimate interaction between user A and movie Avengers when user A has not watched Avengers. Directly estimating it is infeasible, but it is possible to estimate it indirectly with factorization machines model. 

Let us say there is an interaction between user A and Lord of the rings rating 5. And, if there are a lot of users in the dataset that **have seen Avengers and Lord of the rings** and rated positively, then the latent vectors for Avengers and Lord of the rings will be located close to each other and user A whose latent vector is located close to Avengers, will be close to Lord of the rings in the k-dimensional vector space(For simplicity, we are not considering other movies user A has watched but will be considered in actual model) and the model will conclude that user A will like Avengers.

## Complexity

Right off the bat, the equation seemingly has the complexity of $O(kn^{2})$$ with all the pairwise interaction terms. But, the complexity can be dropped to linear time by rearranging the equation, which is shown below.

<img width="552" alt="Screenshot 2024-09-06 at 11 26 24 AM" src="https://github.com/user-attachments/assets/9f1b180d-0a1f-47a9-b37c-d94132f79948">

## Learning

parameters that have to be learned during training are, thus, $(w_{0}, w$ and $V)$ and can be learned by gradient descent methods, namely SGD and with variety of losses that can be used for different purposes. 

<img width="528" alt="Screenshot 2024-09-06 at 11 32 18 AM" src="https://github.com/user-attachments/assets/e0f6026c-f9ae-43e6-99ee-7f4310525a19">

## d-way factorization machine

Even though shown in 2-way for the equation above, FM can be used for d-way interactions, larger than 2.

# FMs vs SVMs

<img width="547" alt="Screenshot 2024-09-06 at 11 37 55 AM" src="https://github.com/user-attachments/assets/70a7866b-3e83-4e13-bfa1-4b4261d9d699">

SVMs, with polynomial kernels, can represent 2-way interaction terms with the model parameter $W^{(2)} \in \mathbb{R}^{n \times n}$. The sheer number of parameters not only increases computational complexity, but also elements in $W{(2)}$ are independent, whereas in FMs, the interaction dot product $<v_{i}, v_{j}>$ and $<v_{i}, v_{l}$ share $v_{i}$ and hence dependent on each other.

<img width="521" alt="Screenshot 2024-09-06 at 11 46 46 AM" src="https://github.com/user-attachments/assets/c0c48ec3-a333-4838-b907-69df13af3727">

in the equation $w_{u,i}^{(2)}$ will can have up to only one observation if we are to say u and i each refers to user and item, continuing with the movie example and most of the interactions in the training set will be 0. And, therefore, even though polynomial SVM contains two-way interaction terms, in actual prediction, it will do no better than linear model without interaction terms. 

# Thoughts upon reading

I read the paper after reading the MF paper and felt that the ability to include other features other than user and item seemed attractive to me. But, it also made me wonder that what if I were to fail to consider all the important features in prediction and it will perform very poorly compared to the MF model. So, in my opinion, the FM model requires a far better understanding in the domain the model is trying to predict with data than the MF model. But, MF model will definitely perform far better if we are to choose features carefully with domain knowledge that we think will influence prediction strongly.

But, as a person who has read papers like VAEs, the lack of non-linearity in the model equation is something that makes the model less attractive.


---

Source: Rendle, S. (n.d.). Factorization machines. https://www.ismll.uni-hildesheim.de/pub/pdfs/Rendle2010FM.pdf 
