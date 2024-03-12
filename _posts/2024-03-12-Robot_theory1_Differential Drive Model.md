---
layout: single
title: "[Robot_theory] Differential Drive model" 
categories: Robot_Theory
typora-root-url: ../
toc: true
use_math: true
---


## Differential Drive Model

Differential drive model(차동 구동 모델)은 로봇 공학 및 제어 이론에서 일반적으로 사용되는 모델 중 하나이다

이 모델에서는 차량이 두 개의 바퀴로 구동되며, 각 바퀴는 개별적으로 제어될 수 있다. 일반적으로 각 바퀴에는 각각의 속도 제어가 가능한 독립적인 모터가 장착되어 있다. 이렇게 각각의 바퀴에 대해 서로 다른 속도를 설정하여 차량의 방향과 속도를 제어할 수 있다. 

차량의 운동을 설명하기 위해 일반적으로 사용되는 표현은 각 바퀴의 '선속도'이다. 하나는 왼쪽 바퀴의 선속도(Vl)이고, 다른 하나는 오른쪽 바퀴의 선속도(Vr)입니다. 이러한 선속도는 각각의 바퀴의 회전 속도를 나타낸다. 따라서 차량의 운동을 제어하려면 각 바퀴의 회전 속도를 조절하여 원하는 방향으로 이동할 수 있다.

![differential model_3](/images/2024-03-12-Robot_theory1_Differential Drive Model/differential model_3.png)

Differential Drive Model을 이용해서 로봇의 움직임을 추적하려면 Euler Integration을 사용하면 된다. 아래 그림과 같이 로봇의 선속도를  x 방향, y 방향의 속도로 나눠서 계산을 한다. 1번, 2번식을 참고하면 된다. 또한 로봇의 각속도를 3번식과 같이 계산하여 t+1 시간의 로봇 위치를 추정할 수 있다. 

![differential model_2](/images/2024-03-12-Robot_theory1_Differential Drive Model/differential model_2.png)





### 각속도 & 선속도

---

Radian은 각의 크기를 나타내는 단위 중 하나이다. 정의는 원의 호(L)를 반지름(r)으로 나눈 값이다. 예를들어서 원의 호의 길이와 반지름의 길이가 같다면 이를 1radian 이라 할 수 있다. 

![원운동_1](/images/2024-03-12-Robot_theory1_Differential Drive Model/원운동_1-1710221115459-6.png)

각속도(w)란, 단위 시간당 회전하는 속도를 의미하며 단위는 라디안/초 이다.  여기서 순간각속도를  계산하기 위해서 단위시간과 각도를 극소로 작게 치환한다면 1번 식이 된다.  여기서 위에서 정리한 라디안 정의 2번 식을 대입해서 아래와 같이 정리하면 3번 식을 도출할 수 있다. 



![원운동_2](/images/2024-03-12-Robot_theory1_Differential Drive Model/원운동_2-1710222783696-11.png)



