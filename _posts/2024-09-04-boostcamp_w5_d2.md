---
title: "부스트캠프 5주 2일차 회고: 피로"
date: 2024-09-04
categories: [Boostcamp,5주차]
tags: [recsys, mult-vae, mcmc, gibbs sampling, influence function]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
math: true
---

저번 주에 이어 이번 주도 recsys에 필요한 머신 러닝 개념들을 배우는 주인데 저번 주도 쉽지 않았지만 이번주는 더 어려웠고 피로감도 좀 쌓이는 것을 느꼈다.
내가 배운 점을 간략하게 정리하고 느낀 점을 적어보겠다.

# MCMC


저번주에 수업이 진행된 VI 대신에 쓸 수 있는 방법론이다. MCMC 는 Markov Chain Monte Carlo의 약자로 결국에 $p(z\mid x)$ 를 구하고 싶은데 $p(x)$ 를 구하기가 굉장히 번거롭기 때문에 구하는 방법을 우회하여 $p(z \mid x)$ 에서 샘플링 하는 방법이다. 기본적인 MC 방법들에는 **rejection sampling**, **importance sampling** 등이 있는데 rejection sampling은 $\tilde{p}(z)$, 즉 $p(z)$의 normalizing constant 를 제외하고도 복잡하기 때문에 샘플링하기 쉬운 $q(z)$를 가지고 와 k배를 취해 샘플링을 하는데 k에 따라서 효율이 결정되고 acceptance rate이 낮아질 수 있는 단점이 있다. Importance sampling 은 expectation을 근사하는데 쓰는 샘플링 기법으로 rejection sampling 과 같이 샘플링하기 쉬운 $q(z)$로 근사하는데 high dimension에서는 잘 작동하지 않는 단점이 있다.


이에 반해 MCMC는 high dimension에서도 비교적 괜찮다. rejection sampling 그리고 importance sampling과 비교해 MCMC가 가지는 장점은 markov chain이 들어감에서 알 수 있듯이 $z^{(2)}$를 샘플링한다고 가정할 때 샘플링된 $z^{(1)}$의 정보를 활용하여 샘플링을 한다는 점이다. Markov chain 에서 stationary 한 distribution이 되는 조건은 $p^{\star}(z)=Tp^{\star}$ 인데 이 때 우리는 우리의 생성모델 즉 $p(z \mid x)$가 이러한 조건을 갖추기 원한다. 


<img width="200" alt="Screenshot 2024-09-06 at 5 24 03 PM" src="https://github.com/user-attachments/assets/0b57ffc2-e4a4-47dd-b1b2-5471e4d92606">

이러한 조건을 만족할 때 detailed balance 상태를 만족하고 stationary 하다. 대표적인 MCMC를 기반한 알고리즘은 **Metropolis Hastings** 알고리즘이 있다. Metropolis 

<img width="200" alt ="Screenshot" src="https://github.com/user-attachments/assets/5da549fc-8a42-4e88-b439-79ce048918c5">

로 샘플링 후보가 있을 때 이 확률 비율이 1보다 크면 무조건 받아들이고 1보다 작으면 그 확률 비율상 후보를 받아들이고 업데이트 하고 proposal distribution이 symmetric 한 데 반해 Metropolis Hastings는

<img width = "200" alt = "Screenshot formula" src = "https://github.com/user-attachments/assets/c61896c1-e1ae-4a5b-845b-858af3452028">

symmetric 한 조건이 빠지지만 proposal distribution에서의 샘플링 확률도 함께 들어간 것을 볼 수 있다. Metropolis Hastings는 고차원에서 위에 언급된 rejection sampling 이나 importance sampling 보다 상대적으로 잘 작동하지만 여전히 slow convergenc 문제가 발생할 수 있는데 이를 해결할 수 있는 다른 방법은 gibbs sampling 이 있다.

**Gibbs sampling**은 굉장히 쉽게 이해가 가능하다. Gibbs sampling 의 원리는 차원마다 sampling 하자는건데 예를 들어 $z^{t}$ 를 업데이트 할 때 conditional distribution 을 이용해 coordinate-wise하게 업데이트하는건데 $z_{1}^{t-1}$ 을 다른 coordinate 들을 고정한채로 sampling 을 하고 업데이트 된 1번째 coordinate 을 넣어서 2번째 coordinate 을 업데이트하고 모든 coordinate 을 업데이트 하고 나면 새로운 샘플이 생성되는 원리다. 따라서, conditional distribution 을 알기 힘든 경우 잘 작동하고 고차원에서 쓰기 좋지만 conditional distribution 을 구하기 힘들다면 쓰기에는 난해한 샘플링 기법이다.

# 마치며

강의 내용을 더 정리하면 좋겠지만 사실 내가 지금 작성한 내용도 내가 제대로 이해하고 작성하는지 확신할 수 없을 정도로 난이도가 있음을 느꼈고 이를 위해 외부 자료도 찾아보고 논문도 읽었는데 피로감이 굉장히 상승함을 느끼고 한계를 느끼기 시작했다. 강도를 조절하는 방법을 고민하는 한 주가 될듯하다.



 
