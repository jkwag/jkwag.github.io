---
title: "LightGCN: Simplifying and Powering Graph Convolution Network for Recommendation_review"
date: 2024-08-12
categories: [Paper,EN]
tags: [LightGCN,collaborative filtering,recsys]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"

---

# Abstract

Not enough evidence on as to why GCN is effective for recommendation and the two major components of GCN, 
feature transformation and nonlinear activation, are found to hinder the performance of collaborative filtering 
and hence recoomendation performance. So, LightGCN, which adopts neighborhood aggregation from GCN, not only does 
improve recommendation performance, but also enhances simplicity of the model, which can be explained from both analytical
and empirical perspectives.

# Major findings
## NGCF
In collaborative filtering, semantic features are lacking only taking an ID as input and hence there is no
good justification to include nonlinear transformations layers for training models, rather interfering with 
model's interpretability. 

Following two experiments, having feature transformation negatively affects model's performance and even though
nonlinear activation function has a slight improvement when feature transformation is present, it negatively affects
the model when feature transformation is not present. And, these results were attributed to the training difficulty, 
rather than overfitting.

## LightGCN
lightgcn aggregates the features of neighbors to represent a target node by weighted sum, capturing the effect of graph convolution
with self-connections

# Things to learn upon reading the paper

GCN, embeddings, collaborative filtering, feature transformation
