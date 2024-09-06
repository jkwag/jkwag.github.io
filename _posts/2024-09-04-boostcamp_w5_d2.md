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
$p(z|x)$ 에서 샘플링 하는 방법이다. 기본적인 MC 방법들에는 **rejection sampling**, **importance sampling** 등이 있는데 rejection sampling은 $\tilde{p}(z)$, 즉 $p(z)$의 normalizing constant 를 제외하고도 복잡하기 때문에 샘플링하기 쉬운 $q(z)$를 가지고 와 k배를 취해 샘플링을 하는데 k에 따라서 효율이 결정되고 acceptance rate이 낮아질 수 있는 단점이 있다. Importance sampling 은 expectation을 근사하는데 쓰는 샘플링 기법으로 rejection sampling 과 같이 샘플링하기 쉬운 $q(z)$로 근사하는데 high dimension에서는 잘 작동하지 않는 단점이 있다.

이에 반해 MCMC는 high dimension에서도 비교적 괜찮다. rejection sampling 그리고 importance sampling과 비교해 MCMC가 가지는 장점은 markov chain이 들어감에서 알 수 있듯이 $z^(2)$를 샘플링한다고 가정할 때 샘플링된 $z^(1)$의 정보를 활용하여 샘플링을 한다는 점이다. Markov chain 에서 stationary 한 distribution이 되는 조건은 $p^{\star}(z)=Tp^{\star}$ 인데 이 때 우리는 우리의 생성모델 즉 $p(z|x)$가 이러한 조건을 갖추기 원한다. 

<img width="200" alt="Screenshot 2024-09-06 at 5 24 03 PM" src="https://github.com/user-attachments/assets/0b57ffc2-e4a4-47dd-b1b2-5471e4d92606">

이러한 조건을 만족할 때 detailed balance 상태를 만족하고 stationary 하다. 대표적인 MCMC를 기반한 알고리즘은 Metropolis Hastings 알고리즘이 있다. 


 
