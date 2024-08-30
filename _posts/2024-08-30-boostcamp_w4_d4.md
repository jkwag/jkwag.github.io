---
title: "부스트캠프 4주 4일차 회고: 집착"
date: 2024-08-27
categories: [Boostcamp,4주차]
tags: [recsys,elbo,em algorithm, variational encoder, generative model]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
math: true
---

이번 주 생성모델 **VAE** 기반 지식인 **ELBO**, **EM algorithm** 을 공부하는데 온 신경을 썼고 **EM Algorithm**에 대한 기반 지식은 이해가 되는데 VAE랑 연결시키는게 아직 힘들긴 하지만 
여기까지 오는데 많은 시간을 썼고 회고 주기적으로 쓰기, 알고리즘 개인공부하기, 논문 블로그에 올리기를 포기하면서 그래도 어느 정도 이해되는 과정에 들어섰다는 것에 만족을 하며 회고를 해보겠다.

# GMM 과 EM Algorithm

VAE -> VI -> EM Algorithm 으로 수업이 진행되면서 점점 머릿속이 혼동으로 가득찬다는 생각이 들었고 그 까닭에 외부 자료를 참고해야겠다고 생각하고 물색하던 중 
앤드류 응의 CS 229 강의 노트에서 VAE 가 등장하여 시도하던 도중, 아쉽게도 강의 영상에서는 직접 안나왔는데 앤드류 응의 강의를 들으니 GMM 부터 시작해서 EM Algorithm을 훑어보고
EM Algorithm을 확대시킨 게 VAE 라는 개념을 숙지하며 공부하니 이해가 훨씬 빨리 됐다. 따라서 앤드류 응의 강의를 간략하게 정리해 보려 한다.

GMM은 Gaussian mixture model의 약자다. 즉 $x$ 의 pdf 가 GMM 을 따른다 가정하면 $x^{(1)} , x^{(2)}, ... , x^{(n)}$ 의 데이타가 있다면 데이타 $x^{i}$ 는 GMM 이 
2개의 Gaussian 을 mixture 라면 $z^{i}$ 라는 데이타는 그룹 1 아니면 2를 나타낼 것이다. 그리고, $z^{i}$ 또한 확률분포를 따른다고 할 수 있는데 이 확률분포를 안다면

$$P(x^{(i)}, z^{(i)}) = P(x^{(i)} \mid z^{(i)}) \cdot P(z^{(i)})$$

를 활용할 수 있으나 아쉽게도 $z^{(i)}$는 관측된 데이타가 아니고 hidden variable 이기에 직접적으로 활용할 수 없다. 만약, $z^{(i)}$ 들을 알면, MLE를 사용할 수 있는데 
$z^{(i)}$가 multinomial을 따른다 하고 parameter가 $\phi$ 라 가정하면 $l(\phi,\mu,\Sigma) = \sum_{i=1}^{m} \log P(x^{(i)}, z^{(i)}; \phi, \mu, \Sigma)$ 로 
$\phi, \mu, \Sigma$ 에 대한 MLE 를 구하면 된다. 

## Jensen's inequality

Jensen's inequality가 EM Algorithm 을 통해 MLE 를 구하는 도구로 사용된다. Jensen's inequality 는 convex 한 함수에 대해 RV X 가 있다면 $f(EX) \leq E(f(X))$ 로 나타내는데

![jensen's inequality](https://www.probabilitycourse.com/images/chapter6/Convex_b.png)

source: https://www.probabilitycourse.com/chapter6/6_2_5_jensen%27s_inequality.php

convex한 function 에 선을 그어보면 쉽게 알 수 있는데 직선의 중심은 당연히 두 점의 평균의 함수값보다 높거나 같다. 그리고, $f(EX) = E(f(X))$ 인 경우는, $X$가 constant 일 때 밖에 없다.
반대로, concave 한 경우, 그림에서 참조하면 알 수 있듯이 반대로  $f(EX) \geq E(f(X))$ 이다.

# EM Algorithm

기반이 완성됐으니 다시 본론으로 돌아가서 log likelihood $l(\theta) = \sum_{i=1}^{m} \log \left( \sum_{z^{(i)}} P(x^{(i)}, z^{(i)}; \theta) \right)$ 에서 $\theta$에
대한 function으로
![em_algorithm](assets/em_algorithm.png) 

EM Algorithm 은 E 스텝에서 **Evidence Lower Bound** 를 알려진 $\theta$에서 tight하게 만들고 M 스텝에서 ELBO를 maximize 하는 $\theta$를 찾고 이를 통해 local maxima에 대한
good approximation 을 찾는데 $ l(\theta) = \sum_{i=1}^{m} \log \left( \sum_{z^{(i)}} Q_i(z^{(i)}) \frac{P(x^{(i)}, z^{(i)}; \theta)}{Q_i(z^{(i)})} \right)$로
임의의 $z^{(i)}$ 에 대한 pdf $Q_i$ 를 정의할 수 있고 $Q$ 가 pdf 이기 때문에 expectation 의 정의에 따라 $\sum_{i=1}^{m} \log \left( \mathbb{E}_{z^{(i)} \sim Q} \left[ \frac{P(x^{(i)}, z^{(i)}; \theta)}{Q_i(z^{(i)})} \right] \right)$
로 정리할 수 있고 이는 Jensen's inequality 에 의해 

$$\sum_{i=1}^{m} \log \left( \mathbb{E}_{z^{(i)} \sim Q} \left[ \frac{P(x^{(i)}, z^{(i)}; \theta)}{Q_i(z^{(i)})} \right] \right)$$

$$\geq \sum_{i=1}^{m} \mathbb{E}_{z^{(i)} \sim Q} \left[ \log \left( \frac{P(x^{(i)}, z^{(i)}; \theta)}{Q_i(z^{(i)})} \right) \right]$$

로 정의할 수 있고 RHS가 ELBO다 . EM Algorithm에서 E 스텝에서 우리는 ELBO 가 최대한 likelihood function 과 tight하길 원하기 때문에 theta 에서 둘이 같기를 원하고 이 때 $\frac{P(x^{(i)}, z^{(i)}; \theta)}{Q_i(z^{(i)})}$
가 constant 여야 되고 이는 $Q_i(z^{(i)}$ 가 $P(x^{(i)}, z^{(i)}$ 에 proportional 해야 됨을 의미하며 $Q$ 는 pdf 이기 때문에 normalization 에 의해 $Q_i(z^{(i)}) = \frac{P(x^{(i)}, z^{(i)}; \theta)}{\sum_{z^{(i)}} P(x^{(i)}, z^{(i)}; \theta)}$
로 Q 를 E 스텝에서 정의할 수 있고 이는 $x^{(i)}$에 conditional 한 $z^{(i)}$의 probability와 동일하다. 총 정리하면 E-step 에서 정해진 $\theta$ value 를 이용해 $Q_i$ 를 posterior 를 이용해 ELBO를 구한 뒤 M-step에서 구한 ELBO에 대해서 MLE를 통해 $\theta$를 업데이트하고
이를 반복해 converge 할 때 까지 구하는데 $\theta$ 값을 중심으로 한다. 

 # VAE

 다시 돌아가서, GMM 과 EM Algorithm에서는 $z$ 가 discrete 하고 1차원으로 가정되어 쉽게 posterior 를 구했지만 VAE의 기반이 되는 $z^(i)$ 들은 continuous 하고 multi dimensional 하기 떄문에 구하기가 쉽지 않고 대신 이를 Q를 posterior 최대한 근사가 되는 값으로 설정하는데
$z$ 를 reparameterize 해서 $z = \mu + \Sigma\epsilon$ 으로 만들어 모델이 학습 가능한 parameter 로 만들어 gradient ascent 방식으로 학습하여 $x$를 출력하는 걸 목적으로 한다고 최종 정리할 수 있다. 즉, EM 알고리즘에서 $\theta$ 만 고려하는 게 아닌 $\mu, \Sigma$ 도 고려한다고
설명할 수 있다.

# 마치며

정말 바쁜 한 주 였다고 회상한다. 수업이 난이도 있게 진행할 줄도 몰랐지만 더 하고자 하는 마음에 논문 구현까지 진행하려 하는데 계획을 지킬 수 있을 지 고민이다. 그러나, 수업이 난이도 있어지면서 일주일 동안 몰입하며 지냈고 어제는 수업 내용이 이해가 가려 하는데 더 실마리가 필요하다 생각해서 다른 할 일도 제쳐두고
수면 시간도 늦추면서까지 앤드류 응의 강의를 들었고 스스로 내용을 이해하고 정리해 블로그에 올릴 수 있는 이해도를 갖출 만큼 좋은 소득이 있었던 것 같다. 이번 주 좋은 결과를 이끌어 냈다 생각하지만 페이스 조절에 대한 부분도 생각해 봐야겠다.


