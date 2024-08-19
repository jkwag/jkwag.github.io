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
![Illustrative example of neighborhood method. If Joe liked 3 movies](/assets/neighborhood.png)
**Latent factor** models are what this paper mainly explains and develop upon. Latent factor models try to characterize both items and users, deduced from users' ratings patterns.  
