---
title: "Matrix factorization techniques for recommender systems review"
date: 2024-08-19
categories: [Paper_review,EN]
tags: [matrix factorization,collaborative filtering,netflix prize, recsys]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"

---

**Disclaimer**
This blog post is a summary of what I understood reading the paper. This is for personal use and might contain wrong information.
___


The paper was released in 2009, which is a long time ago considering I am writing this blog post in 2024. As far as I know, this paper was a major breakthrough in the field of recommendation system by introducing collaborative filtering into the domain. 
There are two major strategies in recommendation system and content filtering tries to profile every user and product with its metadata such as genre and actors for movies, as was discussed in the paper. And, creating profiles every time is such a burdensome 
work even though predictions can be accurate as much as possible when data is available, another strategy had to be adopted that can be easily accessible not giving much of accuracy up, which was collaborative filtering.

# Collaborative filtering

Collaborative filtering tries to analyze user-item relationships based on past user behavior and predict future user-item associations. Collaborative filtering is more often accurate than content filtering with more observed data, however it still suffers from cold start
, relatively performing worse when new products and users are brought into the system. There are two primary methods that can be used for collaborative filtering, neighborhood methods and latent factor models. Neighborhood methods base their analysis on the relationships
between items or users. In the paper, Saving Private Ryan is mentioned as an example for neighborhood methods. Saving Private Ryan's neighbors would be war movies, Spielberg movies and the users who liked Saving Private Ryan is likely to like its neighbors as well.


![Illustrative example of neighborhood method. If Joe liked 3 movies, based on the users that liked the movies Joe liked, we can recommend other movies to Joe with the movies the other users liked.](/assets/neighborhood.png)


**Latent factor** models are what this paper mainly explains and develop upon. Latent factor models try to characterize both items and users, deduced from users' ratings patterns. we can define different dimensions to the model with dimensions we deem influential to users and products.
And, in the paper, matrix factorization is suggested as a method to realize latent factor models.

# Matrix factorization

Matrix factorization basically creates two matrices, one for user-latent factors and the other for item-latent factors. And, predicted ratings can be calculated by the dot product of two matrices
$$\hat{r}_{ui} = \mathbf{q}_i^{\top} \mathbf{p}_u$$ 

where q represents a vector for each item i and p represents a vector for each user. The matrix form reminds of the famous SVD in linear algebra, but due to the large amount of missing values in the matrix, the incomplete matrix makes SVD ineligible. Imputation has been suggested as a solution to the problem too, but the cost was a major problem. Regularized model was therefore an alternative to alleviate the underlying problem. 

\[
\min_{q^*, p^*} \sum_{(u,i) \in K} \left( r_{ui} - q_{i}^\top p_{u} \right)^2 + \lambda \left( \|q_{i}\|^2 + \|p_{u}\|^2 \right)
\]


avoiding overfitting by penalizing on magnitudes of q and p matrices and constant $\lambda\$ controls the extent of regularization. There are two learning algorithms that can be be used for this matrix factorization. Stochastic gradient descent is popularized by Simon Funk updating parameters with the following two formulas. 

$q_{i} \leftarrow q_{i} + \gamma \cdot (e_{ui} \cdot p_{u} - \lambda \cdot q_{i})$
$p_{u} \leftarrow p_{u} + \gamma \cdot (e_{ui} \cdot q_{I} - \lambda \cdot p_{u})$



 
