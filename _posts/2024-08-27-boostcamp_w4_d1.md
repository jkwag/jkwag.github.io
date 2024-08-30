---
title: "부스트캠프 4주 1일차 회고: 갈림길"
date: 2024-08-27
categories: [Boostcamp,4주차]
tags: [recsys,kl divergence, variational encoder, generative model]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
math: true
---

4주차가 시작되었다. 이번 주도 흥미로운 한 주가 될거라고 예상하는데 일단 깃허브 특강을 연속으로 이틀 듣고 드디어 CV NLP Recsys 도메인 별로 각자 나뉘어 각 도메인에 맞춰진 수업을 듣기에 Recsys에 특화된 수업을 듣는다는 게 기대되고 그러기에 제목을
갈림길로 지었다. 일단 이틀동안 있었던 피어세션에 관해 간략하게 적어본다.

# 월,화 피어세션과 논문 리뷰

저번 주에 팀원들과 논의하기로 했던 논문 구현에 대한 문제를 팀원들과 상의했는데 캐글과 논문 구현 중 캐글은 일단 읽고 있던 추천 시스템 논문을 적용할 방법이 딱히 떠오르지 않아 논문 구현에 초점을 두기로 했는데
기대했던 팀원들과 협력하여 논문을 구현한다는게 말처럼 쉽지 않다고 결론이 나 일단은 각자 할 수 있는 데로 구현하고 데이타셋을 구해 성능을 시험하여 서로 비교해 보기로 했는데 경쟁하는 부분에서 동기부여가 되서
결과적으로 나쁘지 않은 방향으로 흘러갔다고 생각한다. 

또한 논문 리뷰가 진행되었는데 Factorization machine 논문을 리뷰하기로 선정하고 주말에 블로그부터 뒤져봤는데 이해가 쉽지 않은 부분이 있어 블로그 5개를 읽고 논문은 정말 대충 읽은 채로, 논문 리뷰에 
들어가게 됐는데, 팀원들과 논문 리뷰를 진행 하던 중 나의 논문 이해가 떨어지는 걸 체감하고 월요일 피어세션 이후 논문을 스스로 1시간 동안 정독했는데 블로그에서는 파악하기 힘들었던 내용들이 눈에 들어왔다.
결국에, 논문을 시작하는 데 있어 블로그가 큰 부담 없이 진입하는 데 큰 도움을 주지만 깊은 이해로 꼭 이어지지는 않는 다는 점을 꺠달았고 이후 논문 정독의 중요성을 꺠달았다.

화요일 피어세션에서는 개인 목표를 서로 공유했는데 이번 주는 깃허브 특강으로 바쁜 일정이 예상되어 얌전하게 금요일까지 릿코드 4문제, mit 알고리즘 강의 2개, 논문 리뷰한 거 블로그 올리기로 정하였다.

# 깃허브 특강

깃허브 특강 1일차에는 **initialize repo, commit, push, branch, head** 등등에 관한 얘기가 나왔는데 branch 부분은 확실히 협업이나 버전 컨트롤을 많이 해 볼 일이 없던 나로서는 굉장히 새로운 부분이였다.
앞으로 팀원들과 프로젝트를 거치면서 협업하는데 원활한 버전 관리 및 실수를 하더라도 안전한 백업이 가능하도록 깃허브를 활용해야겠다.

# 추천 시스템 동향

이번 주 강의는 추천 시스템 중에서도 **generative model, causal inference, causal graph discovery, data valuation** 을 주로 다루는데 1강에서는 추천 시스템의 동향을 소개 및 통계학의 기본을 다뤘는데
**CLT, Likelihood, MLE ** 는 학부 때 주구장창 나왔던 컨셉이기에 무난하게 진행되었다. 추천 시스템 발전 동향을 크게 3개로 나눈게 제일 눈길이 갔는데 Shallow model -> Deep model -> Large scale generative model로
소개되었다. **shallow model**로는 대표적으로 최근 논문 리뷰를 올렸던 행렬 분해로 대표되는 MF 모델, **deep model** 은 neural network 를 활용한 vae 같은 모델, **large scale generative model**은 P5같이 여러개의 
Task 를 하나의 모델로 수행할 수 있는 모델로 소개되었다.

# 생성모델

Supervised learning 의 목표가 x에서 y를 올바르게 추정하는 모델이 목표라면 **Generative model** 의 목적은 (x,y) 나 (x)가 주어졌을 떄 이 샘플 데이타가 속하는 **distribution** 을 추론하는게 목표이다. 셍성 모델이 supervised learning에 비해
가지는 가장 큰 장점은 데이타의 숨겨진 구조를 파악하기 때문에 x 분포의 다른 샘플 데이타가 주어졌을 때 안정적으로 y를 예측하거나 anomaly detection 등 다양하게 활용될 수 있다.

## VAE

VAE는 대표적인 생성모델 중 하나인데 일단 VAE는 variational autoencoder를 축약한 표현이다. ![vae](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*r1R0cxCnErWgE0P4Q-hI0Q.jpeg)

source: [https://medium.com/geekculture/variational-autoencoder-vae-9b8ce5475f68](https://medium.com/geekculture/variational-autoencoder-vae-9b8ce5475f68)

이미지에서 x,$xhat$,z에 집중해서 보면 x가 z 가 있는 layer를 거쳐서 다시 x를 예측하는 값을 뱉어내는데 이 떄 z라는 단은 불필요한 정보를 제거한 압축하는 단이라고 얘기할 수 있다. 여기서 z는 hidden layer에 속해있는 말 그대로 hidden variable,
x와 connection 이 있다 생각하지만 observed data가 아니고 따라서 z의 distribution 을 근사하게 예측할 수 있다면 x 또한 training 을 통해 z로부터 생성이 가능하다.

```math

P(x \mid z) = \frac{P(z \mid x) \cdot P(x)}{P(z)}

```

즉, bayes rule을 이용하여 x와 z의 joint distribution, z의 distribution 을 알면 posterior distribution 을 구할 수 있고 이를 통해 x를 생성 가능하다는 뜻이다. 그러나, 문제는 우리는 z의 distribution 이 뭔지도 모르고 z는 hidden variable 이기 때문에
근사도 불가능하다. 이는 P(X|Z) 를 통해 해결할 수 있는데 다음과 이어서 적어보겠다.

# 마치며

1일차에 이번 주의 커리큘럼을 확인하니 확실히 추천 시스템의 수학적인 근간을 설명하고 있어 쉽지 않아 보이지만 **도전**에 직면했을 떄 내가 **몰입함**을 느끼고 있어 앞으로 기대된다.



