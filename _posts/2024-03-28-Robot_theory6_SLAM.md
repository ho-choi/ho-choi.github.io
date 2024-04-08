---
layout: single
title: "[Robot_theory] SLAM" 
categories: Robot_Theory
typora-root-url: ../
toc: true
use_math: true
---

## SLAM

Simultaneous Localization and Mapping, SLAM 이란 주변 환경을 탐색하고 동시에 자신의 위치와 주변 지도를 작성하는 기술이다. 

미지의 환경에서 정확한 위치를 파악하고 지도를 작성하는데 필수적인 기술이기도 하다. 

SLAM을 수식으로 표현하면 아래와 같다.  



![SLAM_1](/images/2024-03-28-Robot_theory6_SLAM/SLAM_1.png){: .img-width-30 .align-center}

  

즉, 관측값과 제어값이 주어졌을 때,  

- $z_{1:t}$  : 관측값
- $u_{1:t}$  : 제어값

현재 위치와 맵의 확률을 구하는 것이다. 

- $x_{0:t}$  : 위치
- $m$  : 맵

  

위의 식을 바로 계산하기란 매우 어렵다.  따라서 이 식을 Factorization  을 통해서 계산 가능하게 만들어야 한다. 

아래 식으로 변경된 것이 이상할 수 있는데, Rao-Blackwellization 방법을 이용해서 Factorization 한 것이다. 

앞쪽이 Mapping 과정이고, 뒷쪽이 Localization 으로 구분할 수 있다. 



![SLAM_2](/images/2024-03-28-Robot_theory6_SLAM/SLAM_2-1711603300497-5.png){: .img-width-30 .align-center}



### Mapping

---



![SLAM_3](/images/2024-03-28-Robot_theory6_SLAM/SLAM_3.png){: .img-width-30 .align-center}

위 수식을 먼저 생각해보자. $x_{1:t}$ 와  $z_{1:t}$ 가 주어졌을 때 $p(m)$ 을 계산하는 것이다. 

$p(m)$ 이란 무엇일까?  아래 3x3 Grip Map을 생각해보자.

![SLAM_4](/images/2024-03-28-Robot_theory6_SLAM/SLAM_4.png){: .img-width-30 .align-center}

 <br/>

$p(m)$이란  채워져 있을 확률을 의미한다. 채워져 있다는 것은 장애물이 있는 것이라고 생각해도 좋다. 

비어 있으면 확률이 0, 채워져 있으면 1로 표현할 수 있다. 이를 Probability of Occupancy라고 정의한다. 



![SLAM_5](/images/2024-03-28-Robot_theory6_SLAM/SLAM_5.png){: .img-width-30 .align-center}

우리가 찾고 싶은 3x3 맵의 Probability of Occupancy는 $p(m) = p(m_{1}) p(m_{2}) \cdots p(m_{9})$ 으로 정의할 수 있다. 

맵의 크기가 커지면 i가 커지면 된다. 맵이 커지게 될 경우, 1보다 작은 수를 여러번 곱하게 되는 형태가 된다. 이럴경우 결과값이 무수히 작은수가 계산되기 때문에 아래와 같이 수식을 정리한다. 



![SLAM_6](/images/2024-03-28-Robot_theory6_SLAM/SLAM_6.png){: .img-width-30 .align-center}

<br/>

우리는 위 수식을 통해서 $p(m_{i})$ 를 가지고, $p(m)$ 을 구할 수 있음을 확인하였다. 그렇기 우선 우리는 $p(m_{i})$ 를 구하는 식을 정리해보자.

아래 그림과 같이 분해할 수 있다. 

![SLAM_9](/images/2024-03-28-Robot_theory6_SLAM/SLAM_9.png){: .img-width-half .align-center}

  

<br/>

분해 한 것을 아래 그림과 같이 식을 풀어 가고, 수식 마지막은 Bayesian Networks 를 이용해서 $z_{1:t-1}$ 조건부를 생략할 수 있다. 

![SLAM_8](/images/2024-03-28-Robot_theory6_SLAM/SLAM_8-1711615839829-16.png)



<br/>

위에서 $z_{1:t-1}$ 을 삭제하고 정리한 식을 이어서 정리하자면 아래와 같다. 

수식이 복잡해 보일 수 있지만, 조건부 확률과 Bayesian Networks를 이용한 것이 전부이다.  식을 최종 정리한 식은 수식 제일 밑에 있다.

![SLAM_10](/images/2024-03-28-Robot_theory6_SLAM/SLAM_10-1711615845605-18.png)

<br/>

위 최종식에서 우리가 얻을 수 있는 것은 왼쪽항에서 $p(m_{i})$ 를 구할 때, 오른쪽 항  $x_{1:t-1}$, $z_{1:t-1}$가 주어진다면 계산할 수 있다는 것 직관이다. 이것은 재귀적으로 맵을 구할 수 있음을 의미한다. 

오른쪽 항들을 어떻게 구할지 아래 단계를 생각해보자

### Occupancy or Empty

---







