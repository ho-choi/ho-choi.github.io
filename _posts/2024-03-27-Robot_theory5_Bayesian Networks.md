---
layout: single
title: "[Robot_theory] Bayesian Networks" 
categories: Robot_Theory
typora-root-url: ../
toc: true
use_math: true
---

## Bayesian Networks



Bayesian Networks는 확률적인 관계를 그래픽으로 모델링 한 것이다. 그래프는 노드(Node)와  엣지(Edge)로 이뤄져 있으며 아래와 같이 구성되어 있다.

- Node : 확률변수
- Edge : 확률변수들 간 관계



---

확률변수가 A, B, C가 존재하고   A, B는 서로 독립적이지만 C의 확률에 있어서 관여한다고 가정해보면 그래프는 아래와 같이 그려진다.



![joint_probability_6](/images/2024-03-27-Robot_theory4_Bayesian Networks/joint_probability_6-1711516471529-9.png){: .img-width-70 .align-center}



---

다음으로, A와 B가 C의 결과에 영향을 미치는 관계라면, 아래와 같이 정리할 수 있다. 

A와 B는 독립적인 관계이기 때문에, 조건부에서 생략해도 무방하다. 

![joint_probability_7](/images/2024-03-27-Robot_theory4_Bayesian Networks/joint_probability_7-1711516513707-11.png){: .img-width-70 .align-center}

---

아래와 같이 복잡한 Network 관계를 갖는다면, 이는 아래와 같이 정리할 수 있다. 

식을 정리해 나가는 과정이 복잡해 보이지만, 결과만 본다면 추론하기 쉽다.

예를 들어서 P(F)의 확률에 영향을 미치는 노드 D만 조건부에 들어가는 것이다. 또다른 예로 P(E)의 확률은 조건부에 A,C만 넣으면 된다.



![joint_probability_8](/images/2024-03-27-Robot_theory4_Bayesian Networks/joint_probability_8-1711516575312-13.png){: .img-width-70 .align-center}



---

### Markov Chain

마르코프 체인은 A, B, C  가 서로 순차적으로 영향을 미치고 있다면, A와 C는 독립적인 관계라고 생각할 수 있다. 

![joint_probability_9](/images/2024-03-27-Robot_theory4_Bayesian Networks/joint_probability_9.png){: .img-width-half .align-center}



---

### Markov Process

마르코프 프로세스는 아래와 같이 직전 State만이 현재 State에 영향을 미친다는 개념이다.

![joint_probability_10](/images/2024-03-27-Robot_theory4_Bayesian Networks/joint_probability_10.png){: .img-width-half .align-center}

