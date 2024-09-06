---
title: "부스트캠프 5주 2일차 회고: 피로"
date: 2024-09-04
categories: [Boostcamp,4주차]
tags: [recsys, mult-vae, mcmc, gibbs sampling, influence function]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
published: false
math: true
---

저번 주에 이어 이번 주도 recsys에 필요한 머신 러닝 개념들을 배우는 주인데 저번 주도 쉽지 않았지만 이번주는 더 어려웠고 피로감도 좀 쌓이는 것을 느꼈다.
내가 배운 점을 간략하게 정리하고 논문 구현을 시도한 경험에 대한 얘기도 하고 소감도 허심탄회하게 풀어보겠다.

# MCMC

저번주에 수업이 진행된 VI 대신에 쓸 수 있는 방법론이다. MCMC 는 Markov Chain Monte Carlo의 약자로 결국에 $p(z|x)$ 를 구하고 싶은데 $p(x)$ 를 구하기가 굉장히 번거롭기 때문에 구하는 방법을 우회하여
$p(z|x)$ 에서 샘플링 하는 방법이다.
 
