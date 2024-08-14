---
title: "네이버 부스트캠프 2주 2,3일차 회고"
date: 2024-08-12
categories: [Boostcamp,2주차]
tags: [boostcamp,neural network,back propagation, classifier, loss function]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"
---

2일차 회고를 올렸어야 됐는데 과제를 하던 도중 고민해야 될 문제가 생겨 미쳐 까먹고 올리지 못해 불가피하게 2일치 학습 회고를 한꺼번에 올린다. 바빠서 회고를 올리는 것을 까먹을 만큼 공부한 주제가 많았다 생각하는데 학습 회고를 하면서 다시 정리해 보고자 한다.
주요 학습한 내용은 기초적으로 뉴럴 네트워크를 트레이닝하는 과정부터 시작하여 NLP 모델인 **RNN**, **LSTM**, **Attention**, **Transformer** 순으로 빠르게 학습을 진행해 나갔다.

# 2일차 학습 내용

기본적인 linear 모델을 지나 input에 linear transformation 을 취한 다음 nonlinear activation function을 몇 layer에 걸쳐 적용시키는 학습 방법인 뉴럴 네트워크부터 시작했는데 기하학적인 justification이 참고할 만 했는데 nonlinear activation function
없이는 classfication boundary가 linear 하기 때문에 복잡한 형태의 데이타에는 부적절 하다는 내용이었다. 그래서 feature transformation하여 함수에 feed하면 더 좋은 성능을 발현할 수 있는데 이를 end to end 로 구현한게 neural network다. 그리고 neural network
에서 제일 중요한 개념 중 하나가 등장하는데 바로 backpropagation이다. backpropagation은 뉴럴 네트워크가 기본적으로 학습 가능한 모델이 될 수 있게 하는 핵심적인 개념인데 미적분의 chain rule 에 기반한다. 먼저 input 을 이용해 forward pass를 하고 우리가 알고 싶은 parameter인
weight 과 bias를 loss에 대해 미분을 여러번 하는 과정을 통해 아무리 복잡한 모델 구조를 갖고 있더라도 기울기를 구할 수 있다.  
![back-pro](/assets/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f313630302f312a71314d374c47694454697277552d344c634671375f512e706e67.png) _example of back propagation_
