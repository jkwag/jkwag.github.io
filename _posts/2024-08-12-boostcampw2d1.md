---
title: "네이버 부스트캠프 2주 1일차 회고"
date: 2024-08-12
categories: [Boostcamp,2주차]
tags: [boostcamp,linear regression,machine learning, classifier, loss function]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"

---
주말을 지나 2주차가 되었다. 오늘 하루 꽤 생산적인 날이었던 것 같다. 논문을 팀원들과 논의하였고 논문을 구현한 코드도 훑어봤고 수업도 무리없이 진행하고 코드도 잘 됐다. 모더를 
맡은 주인만큼 바쁘게 지나가겠지만 그만큼 보람찬 주로 만들 수 있고 이번 주 목표는 개인 공부 습관 시작하기가 우선적인 것 같다.

## 강의 정리

오늘은 머신 러닝 라이프사이클과 기본적인 선형 회귀와 loss function 과 optimization 등등 머신러닝의 기본 요소에 집중한 강의들을 들었다. 머신 러닝 라이프사이클에서 흥미
있던 내용은 단연코 과제를 <T,E,P> Task, Experience, Performance 로 정의할 수 있다는 것이였다. 막연히 컴퓨터가 스스로 학습하는 것을 머신 러닝이라고 정의하고 넘어가던
나에게 구체적인 과정의 도식을 머릿 속에 새길 수 있는 키워드였다. 전체적인 머신 러닝 라이프사이클을 훑는 것은 앞으로 기업 실무에 들어갔을 때 내가 라이프 사이클의 어떤 부분을 
맡게 될지, 그리고 앞으로 있을 해커톤에서 어떻게 하나의 전체적인 과정을 스무스하게 팀원들과 진행할 수 있을지 생각해 보게 만드는 내용이었다.

또한, Supervised learning 중 **Nearest Neighbor Classifier** 는 predict의 complexity가 O(N)이기 때문에 비효율적인 학습 방법이라는 점도 흥미로웠다.
알고리즘의 효율성과 머신 러닝의 연결 고리에 대해 생각해 보지 않던 나에게 그 필요성을 체감하게 되는 계기였다.

Linear classifier에서 output을 softmax로 출력해서 0에서 1사이의 값으로 보여준다는 것도 흥미로웠는데 특히나 softmax를 쓰는 justification으로 결과값이 0 에서 1사이이고
classifier의 점수 차가 커질수록 확룰이 커지는 원리로 sigmoid를 가져오면서 확률이 될 수 있는 조건을 만족시킨다는게 학부에서 배웠던 지식을 활용할 수 있어서 좋았다. 최적화
부분서 gradient descent, stochastic gradient descent 는 가장 큰 차이가 computational cost 부분이라 이해했는데 머신러닝서 computational difficulty를
항상 고려해야 된다는 점을 상기받았다.

## 피어세션

논문 리뷰와 강의에 관한 얘기를 했다. 머신 러닝 라이프 사이클과 관련해 팀원들이 진행했던 얘기를 할 수 있어서 좋았는데 프로젝트를 진행하면서 아쉬웠던 점, 다음에 다시 하면
발전시킬 수 있는 점을 논의해 보면서 역시나 프로젝트를 하면서 정말 다양한 부분이 고려되어야 된다는 인상을 받았다. 또한, kNN 과 k Means 방식의 차이점에 대해서 이야기를 
나누었는데 두 방법은 이름에서 알 수 있듯이 비슷해 보이는 방법이지만 kNN은 레이블이 주어진 Supervised learning이고 k Means는 레이블이 주어지지 않은 Unsupervised
learning 이라는 극명한 차이가 있다.

## 마치며

논문 이외에도 개인 공부를 어떻게 진행할까 생각하는데 코딩 테스트 연습과 컴퓨터 사이언스 이론 공부를 병행할 것 같다. 오늘은 leetcode서 몇 문제 풀어보고 하루를 마칠 것 같다.
내일도 화이팅



