---
title: "Matrix factorization techniques for recommender systems review"
date: 2024-08-19
categories: [Paper_review,EN]
tags: [matrix factorization,collaborative filtering,netflix prize, recsys]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"

---
___
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

Minimize over $\( q^* \) and \( p^* \):$

$\[
\sum_{(u,i) \in K} \left( r_{ui} - q_{i}^\top p_{u} \right)^2 + \lambda \left( \|q_{i}\|^2 + \|p_{u}\|^2 \right)
\]$


avoiding overfitting by penalizing on magnitudes of q and p matrices and constant $\lambda\$ controls the extent of regularization. There are two learning algorithms that can be be used for this matrix factorization. Stochastic gradient descent is popularized by Simon Funk updating parameters with the following two formulas. 

$q_{i} \leftarrow q_{i} + \gamma \cdot (e_{ui} \cdot p_{u} - \lambda \cdot q_{i})$

$p_{u} \leftarrow p_{u} + \gamma \cdot (e_{ui} \cdot q_{I} - \lambda \cdot p_{u})$

with loss function simply subtracting the predicted rating from the actual rating. The above two formulas modify parameters proportional to $\gamma$. Also, alternating least squares can be used assuming one of qi's or pi's being constant and the problem becomes quadratic and solvable by least squares. Advantages alternating least squares has to stochastic gradient descent is parallelization and ones with implicit data, since matrices with implicit data will no more be deemed sparse. 

## Adding biases

adding bises can further improve the method and it makes sense that some users innately give ratings better than other users and some items just attract users more and adding biases can be decomposed into two parts: item bias and user bias. 
$b_{ui} = \mu + b_{i} + b_{u}$
and the above bias formula is added to the previous formula to calculate the predicted rating and loss function. 

## Additional inputs

Authors also suggest incorporating additional information about users, especially implicit feedback, namely purchase history and browsing history. And, additional inputs are inputted in boolean values and $N(u)$ denotes the set of items user u implicitly expressed interest in. Each item in N(U) is associated with a vector in f-dimensional space and the user's preference for items can be characterized by 
$\sqrt{|N(u)|} \sum_{i \in N(u)} x_{i}$ and user attributes can be additionally inputted in the same way denoted by $y_{a}$. 

## Temporal dynamics

Authors pointed out the explained models in the previous sections are all static, failing to explain temoporal changes in user preferences, item biases, and user biases. Intuitively, users might change their taste according to time as trends change continuously. Also, users can rate products higher through time such as a user giving 3 out of 5 giving 4 out 5 later can happen. Also, item's popularity can fade over time due to various factors. But, the author does not take temporal factor into account for q matrix, since items are static if we are to consider item - latent factor relationships.

## Inputs with varying confidence levels

Authors aruge that not all ratings should be used for analysis with equal weight or confidence. For example, massive advertising can interfere with user's true preference when advertising does not influence much. It is suggested that confidence levels can be calculated from available numerical values such as the frequency of actions from users including number of clicks. So, the cost function is further modified by multiplying $c_{ui}$ term to the summationon which completes the following final formula in the paper.

$\min_{(p, q, b)} \sum_{(u,i) \in K} c_{ui} \left( r_{ui} - \mu - b_{u} - b_{i} - p_{u}^\top q_{i} \right)^2 + \lambda \left( \|p_{u}\|^2 + \|q_{i}\|^2 + (b_{u})^2 + (b_{i})^2 \right)$

# Results

With the above model, they were able to improve about 9.46 percent compared to Netflix's system. Factorizing the Netflix's user-movie matrix helped find the most useful dimensions for predicting preferences as shown in the image below. 

![The graph represents the most important two dimensions in predicting user-movies relationships and as can be seen in the graph, movies that are close together have shared attributes and users are likely prefer movies that belong to one cluster in the graph.](/assets/netflix-matrix.png)

The models' performance was measured by RMSE and as was expected from the explanations above, the model with temporal dynamics was able to achieve the smallest RMSE with RMSE = 0.8563. Also, within the same model, increasing number of parameters improved model's performances attributed to increasing model's dimensionality. Also, it was observed that taking temporal dynamics into account in the model was the most significant improvement in accuracy.

# Final Thoughts

For me, the most impressive part in the paper was to include temporal factors in the model and the justification totally made sense since our preference is quite volatile and we can like one thing today and suddenly put our mind away from it tomorrow. I also wondered how exactly they implemented temporal dynamics in their model but it was just represented as time functions unfortunately. Fianally, as pointed out in the LightGCN paper, I think the method can further be improved by considering user-user relationships and item-item relationships in the model.  

___
Source: Y. Koren, R. Bell and C. Volinsky, "Matrix Factorization Techniques for Recommender Systems," in Computer, vol. 42, no. 8, pp. 30-37, Aug. 2009, doi: 10.1109/MC.2009.263.
keywords: {Recommender systems;Motion pictures;Filtering;Collaboration;Sea measurements;Predictive models;Genomics;Bioinformatics;Nearest neighbor searches;Computational intelligence;Netflix Prize;Matrix factorization},
___ 
