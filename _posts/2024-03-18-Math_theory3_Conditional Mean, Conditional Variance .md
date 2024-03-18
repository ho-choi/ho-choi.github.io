---
layout: single
title: "[Math_theory] 조건부 평균, 조건부 분산" 
categories: Math_Theory
typora-root-url: ../
toc: true
use_math: true
---



## 평균





## 조건부 평균

Arithmetic Average 이라 불리는 산술적 평균은, 우리가 알고 있는 방법대로 데이터를 다 더하고 이를 샘플 수 로 나눠주면 평균이 된다.

![arithmetic average](/images/2024-03-18-Math_theory2_Mean, Variance/arithmetic average.png)

하지만, 데이터가 발생하는 빈도가 존재한다면, 이를 평균을 구하기 위해서는 아래와 같이 계산되어야 한다. 

데이터에 빈도수(w)를 곱해주고 이를 전체 횟수로 나눠주면 이것이 평균이 된다.  이를 이산적인 확률변수 (Discrete Random Variable) 관점에서 생각해 보면 아래와 같이 아래와 같이 계산할 수 있다. 



![Expectation](/images/2024-03-18-Math_theory2_Mean, Variance/Expectation.png)



여기서 확률변수가 연속 확률변수 일 경우라면, $P(x_i)$ 를  $f_x(x) dx$ 로 바꿔주면서 적분하면 아래와 같은 식이 된다.

![Expectation_continuous](/images/2024-03-18-Math_theory2_Mean, Variance/Expectation_continuous.png)





## 분산

Variance 관련 글을 보다 보면, Central Moments 라는 표현을 보게 된다. 무게 중심이라는 말인데 Central Moments는 아래와 같이 정의 된다. 

확률변수에서 위에서 정의한 평균을 빼고 n 제곱한 것의 평균으로 정의한다.  위에는 Discrete RV, 아래는 Continuous RV를 의미한다.

![moments](/images/2024-03-18-Math_theory2_Mean, Variance/moments.png)



여기서, n=2 일 경우는 중요한 의미를 같기에 이를 Variance, 분산이라 정의하는 것이다. 

![variance](/images/2024-03-18-Math_theory2_Mean, Variance/variance-1710738172054-7.png)



컴퓨터 과학 (Computer Science , CS) 관점에서는 연산 속도 문제로 인해 아래와 같이 분산을 계산하는 것이 일반적이다. 

![variance_second](/images/2024-03-18-Math_theory2_Mean, Variance/variance_second.png)

