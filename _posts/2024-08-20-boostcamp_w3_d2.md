---
title: "네이버 부스트캠프 3주차 1일 차 회고"
date: 2024-08-20
categories: [Boostcamp,3주차]
tags: [boostcamp,data scientist,visualization, eda]
toc: true
toc_label: "Table of contents"
toc_icon: "cog"

---

3주차가 시작되었다. 2주차 마지막날은 할 일을 다 끝내고 개인 공부, 조별 공부를 진행하느라 회고를 쓰지 않았고 3주 1일 차 회고에 묶어서 쓰기로 했다. 3주차의 테마는 **EDA** 그리고 **Data visualization** 인데 쉬운 것 같아 보이면서도 진행하면서 예상치 못하게 소중한 insight를 얻을 수 있기에
주의깊게 진행해야 되는 과정이라 생각하고 강의에서도 이러한 부분이 강조되었다. 첫 강의는 비즈니스에서 **Data Scientist** 를 비롯한 Data 직무가 비즈니스에서 가지는 가치와 어떻게 비즈니스에 기여를 할 수 있는지에 대한 부분인데 인상 깊게 들었고 이 대목에도 글을 진행하면서 다뤄보겠다.

# 월요일 피어세션

월요일 피어세션에서는 읽기로 했던 논문 **matrix factorization techniques** for recommender systems 를 팀원들과 함꼐 리뷰했다. 조금 오래된 논문이지만 recsys 도메인에서 기본으로 쓰였던 모델인만큼 읽으면서 인상이 깊었고 팀원들도 그 부분에 공감을 했다. 주로 얘기됐던 부분은 user-item matrix는
sparse 하기 때문에 matrix를 3개로 분해하는 SVD 가 불가능하다는 점, 그러기 때문에 user-latent factor, item-latent factor 두 개로 분해하면서 regularization term들을 추가해 overfitting을 방지했다는게 핵심 내용으로 거론되었고 결과적인 부분에서는 temporal effect가 가장 컸다는 것.
그러나 논문에서는 이러한 function term들을 b(t), q(t) 이런식으로 정확히 temporal effect가 어떻게 반영됐는지에 대한 부분이 생략되었기에 아쉬웠다. 또한, **트랜스포머** 구조서 encoder 와 decoder 부분에 대해 제대로 아직 숙지되지 못한 듯 하여 팀원들에게 질문하면서 설명하려 하였고 이 부분에서 부족한 점을 발견하여
다시 공부할 필요성을 느끼고 이번 주 안에 다시 볼 계획이다.

# 월요일 계획 및 개인 공부

개인공부를 혼자서 하니까 동기부여가 잘 생기는 것 같지 않아 팀원들과 개인 공부 계획을 서로 공유하고 주간의 마지막에 서로의 진행상황을 체크하기로 했다. 나는 주로 알고리즘과 코딩 테스트 준비가 개인 공부의 주이고 이를 팀원들에게 알렸다. 월요일에 개인 공부 할 여유가 꽤 많았는데 알고리즘 공부를 하던 도중
그래프 이론이 부족하다는 생각이 들었고 그래프 이론 보충이 개인 공부 최우선 과제가 되었다. 코딩 테스트 문제는 일주일에 최소 3 개 풀기가 목표이다.

# 화요일 3주차 시작

화요일 


