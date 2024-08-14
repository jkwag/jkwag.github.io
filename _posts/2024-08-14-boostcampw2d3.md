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


![back-pro](/assets/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f313630302f312a71314d374c47694454697277552d344c634671375f512e706e67.png) _example of back propagation source:https://github.com/Vercaca/NN-Backpropagation?tab=readme-ov-file_


위의 이미지와 같이 forward된 식을 역으로 추적해 upstream gradient(이전 단계에서의 gradient)와 local gradient의 product로 원하는 parameter의 기울기를 각각 구할 수 있다. 그 외에 또 neural network를 구성하는 요소로는 activation function, weight initialization, learning rate 등등이 있는데
activation function에서는 특히 학교에서 프로젝트를 진행할 때 구글링으로 단순히 relu 가 좋다는 결과만을 보고 그대로 가져다 썼는데 상세한 이유인 sigmoid나 tanh보다 연산이 빠르지만 zero centered 되어 있지 않고 x = 0일때 미분 불가능하다는 걸 알아서 좋았고 tweak을 가해 어느 정도 보정된 방법도 배웠다.
initialization에서는 보통의 initialization 방법으로는 층을 깊게 쌓을수록 gradient들이 0에 오밀조밀하게 distribute 된다는 점을 볼 수 있었는데 xavier initialization은 비교적 층을 깊게 쌓아도 gradient data가 전체적으로 0으로 수렴하는 속도가 느린 점을 볼 수 있었고 learning rate은 empirical 하게 
정하면 된다고 마스터(강의자)의 조언이 있었는데 본인의 노하우도 공유하면서 2일차는 마쳤다.

# 3일차 학습 내용

3일차는 Transformer까지 향하면서 이 주차의 학습을 마무리 했는데 가는 과정이 꽤 고통스러웠던 것 같다. RNN부터 시작하는 수업이었는데 Time series data를 처리하기 위한 모델이기 때문에 가변적인 길이의 sequence가 input으로 들어갈 수 있다는게 가장 큰 장점으로 보였다. 그러나, gradient를 모든 층이 공유하기 때문에 1이 아닌 gradient가 
설정되어 있다면 vanishing 하거나 (0에 수렴하는) 아니면 exploding(폭발적으로 증가하는) 문제가 치명적으로 느껴졌었다. 또, RNN은 입력과 출력의 길이가 같아야 하는데 강의에서는 seq 2 seq 모델이 해결했다고 추후 설명되었다. 

gradient가 power 형태로 변하기 때문에 발생하는 문제인 long term dependence 문제를 해결한 모델로는 lstm이 대표적인데 cell state라고 하는 hidden state와 
highway(말 그대로 skip해 지나가는) 방식을 도입하여 과거 정보를 얼마나 들여올지 그리고 얼마나 업데이트를 할 지 등을 정하게 된다. 따라서, forget gate와 input gate 를 통해 long-range information 즉 훨씬 더 과거의 정보를 RNN에 비해서 출력값에 더 많은 영향을 끼친다 볼 수 있다. G

RU는 LSTM과 유사하지만 FC단에서 두 갈래로 나뉘어
하나는 cell state용으로 사용하는 LSTM의 변형 형태인데 컴퓨팅 파워가 부족한 경우 GRU가 LSTM보다 권장된다. 

seq2seq 모델은 RNN 기반 모델은 인풋과 아웃풋 사이즈가 같아야 되는 언어 모델에서는 당연히 치명적인 단점을 보완한 모델인데 Encoder 와 Decoder를 사용해 encoder 단에서 각 단계서 output 과정을 거치지 않고 전체적인 hidden vector를 배워 임베딩하고 이 임베딩을 Decoder가 받아 auto-regressive하게 문장을 출력한다. 특이점은 Decoder 단에서 Teacher forcing이라는 테크닉이 사용되는데 $y\hat{}$ 이 인풋으로 들어가는게 아니라 $y_(t-1)$이 대신 사용되는데 학습단계이기 때문에 그렇다. 다음은, 어텐션 모델인데 트랜스포머 모델로 향하는 길목에 위치하는 모델이라 볼 수 있다.

RNN 을 기반으로 하는 모델은 구조 특성상 정보 손실이 발생하는데 어텐션 모델은 input token 의 모든 hidden state 을 고려하는데 직접적인 Attention(Q,K,V) 값을 계산한다. Q 는 query, K 는 Key, V는 Value를 의미함으로서 query가 비교하는 기준, key가 비교하는 대상의 기준, value가 비교하는 대상의 값으로서 출력값은 weighted sum으로 보통 나타내는데 다른 방법으로 나낼 수 있고 weight단을 추가해 학습 가능하게 설정할 수도 있다.  

트랜스포머 모델은 chatgpt의 근간이 되는 model structure라고 강의서 설명이 나왔을 때 감회가 나름 새로웠다. 구글에서 나온 Attention is all you need 에서 시작됐는데 인풋으로 들어가는 elemenet들은 서로 유기적으로 관련되어 self-attention을 통해 context를 배우는게 기본 아이디어다. 구조적으로는 Attention(Q,K,V)라는 함수로 향하는 과정에 W를 추가하는데 각각의 Q,K,V에 대해 각각의 W를 적용한다. 그리고 트랜스포머 블록을 지나서 어텐션 임베딩 단을 향할때도 또 다른 W를 적용하여 이를 여러 층 쌓아 가까운 input token끼리 얼마나 연관되어 있는지 확인하는 과정을 거치는데 이를 contextualization 이라고 부르기도 한다.  

# 과제

이번 과제는 저번 주 과제와 다르게 난이도가 꽤 어렵게 느껴지고 시간도 그만큼 좀 소모가 되었는데 주어진 수식의 backpropagation 과정을 pytorch 를 가져다 안에 있는 메서드를 이용하는게 아니라 scratch 부터 만드는데 chain rule을 하나하나 적용하다 보니 헷갈리는 부분이 있었다. 특히나, 마지막 activation function인 softmax의 gradient를 구하는게 난해했는데 [이 링크의]([https://towardsdatascience.com/derivative-of-the-softmax-function-and-the-categorical-cross-entropy-loss-ffceefc081d1](https://www.michaelpiseno.com/blog/2021/softmax-gradient/)) 의 설명을 통해 쉽게 해결되었는데 요약하자면 softmax의 loss function인 cross entropy 에서 y가 one-hot vector 라는 점을 이용해 단순화시켜 체인룰을 통해 yhat-y가 softmax단에서의 gradient라는 점이 제일 인상 깊었고 풀었을 때 쾌감을 느꼈다. 또, 과제에서 hyperparameter를 내가 임의로 설정해서 모델을 테스트해보는 시간도 있었는데 운 좋게 좋은 결과가 바로 나왔지만 이에 대한 고찰이 더 필요하다고 느꼈다.

# 마무리
피어세션과 마스터 세션, 그리고 튜터링 시간에 대해서 풀어써야 하지만 워낙 학습 내용이 방대했고 스케쥴이 바빴기에 주간 마무리하는 회고에 적을 듯 하다. 특히나 이미 강의를 다 들어 어느 정도 분배하는 차원도 있다. 총평하면 이틀동안 배운 학습 내용이 방대했고 이렇게 학습 회고를 쓰고 나서도 다시 한 번 더 훑어 봐야겠다는 다짐을 하게 된다. 나 스스로에게 수고 많았다 얘기하고 싶고 내일 쉬고 주간 마무리 회고로 돌아오겠다.
