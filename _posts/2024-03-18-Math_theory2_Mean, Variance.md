---
layout: single
title: "[Math_theory] 평균과 분산" 
categories: Math_Theory
typora-root-url: ../
toc: true
use_math: true
---



## 평균

Arithmetic Average 이라 불리는 산술적 평균은, 우리가 알고 있는 방법대로 데이터를 다 더하고 이를 샘플 수 로 나눠주면 평균이 된다.

![arithmetic average](/images/2024-03-18-Math_theory2_Mean, Variance/arithmetic average.png)

하지만, 데이터가 발생하는 빈도가 존재한다면, 이를 평균을 구하기 위해서는 아래와 같이 계산되어야 한다. 

데이터에 빈도수(w)를 곱해주고 이를 전체 횟수로 나눠주면 이것이 평균이 된다.  이를 이산적인 확률변수 (Discrete Random Variable) 관점에서 생각해 보면 아래와 같이 아래와 같이 계산할 수 있다. 



![Expectation](/images/2024-03-18-Math_theory2_Mean, Variance/Expectation.png)



여기서 확률변수가 연속 확률변수 일 경우라면, $P(x_i)$ 를  $f_x(x) dx$ 로 바꿔주면서 적분하면 아래와 같은 식이 된다.



![Expectation_continuous](/images/2024-03-18-Math_theory2_Mean, Variance/Expectation_continuous-1710808435839-1.png)

### 선형변환된 확률변수의 평균

지금까지는 이산적/연속적 확률변수 X 에 대한  평균을 정의했다. 확률변수 $X$가  $aX + b$ 로 선형변환을 할 경우 평균을 정의하면 아래와 같다. 



![Expectation_linear transfer](/images/2024-03-18-Math_theory2_Mean, Variance/Expectation_linear transfer.png)





### 확률 벡터의 평균

확률 벡터란, 원소가 모두 확률변수인 벡터를 의미한다. 아래와 같이 U와 W는 확률 변수이고 이들의 집합인 X는 확률 벡터다.



![random vector](/images/2024-03-18-Math_theory2_Mean, Variance/random vector.png)

이러한 확률 벡터의 평균은 아래와 같이 정의할 수 있다. 



![Expectation_linear transfer_vector](/images/2024-03-18-Math_theory2_Mean, Variance/Expectation_linear transfer_vector.png)







## 분산

Variance 관련 글을 보다 보면, Central Moments 라는 표현을 보게 된다. 무게 중심이라는 말인데 Central Moments는 아래와 같이 정의 된다. 

확률변수에서 위에서 정의한 평균을 빼고 n 제곱한 것의 평균으로 정의한다.  위에는 Discrete RV, 아래는 Continuous RV를 의미한다.

![moments](/images/2024-03-18-Math_theory2_Mean, Variance/moments.png)



여기서, n=2 일 경우는 중요한 의미를 같기에 이를 Variance, 분산이라 정의하는 것이다. 분산에 대한 의미는 확률변수에서 평균을 빼고 이를 제곱한 것의 평균을 의미한다. 

![variance](/images/2024-03-18-Math_theory2_Mean, Variance/variance-1710738172054-7.png)



컴퓨터 과학 (Computer Science , CS) 관점에서는 연산 속도 문제로 인해 아래와 같이 분산을 계산하는 것이 일반적이다. 

![variance_second](/images/2024-03-18-Math_theory2_Mean, Variance/variance_second.png)

### 선형화된 확률변수의 분산

위에서 분산을 정의한 것 처럼, 확률변수에서 평균을 빼고 이를 제곱한 것의 평균이다. 이처럼 선형변환 한 $aX +b$ 가 확률변수라면 분산은 아래와 같이 정의가 된다. 



![Variance_linear](/images/2024-03-18-Math_theory2_Mean, Variance/Variance_linear.png)

위 식에서 $E(aX +b)^2$  는 선형화된 확률변수의 평균에서 $aE(X)+b$ 임을 증명하였다. 따라서 아래와 같이 정리할 수 있다. 



![Variance_linear_2](/images/2024-03-18-Math_theory2_Mean, Variance/Variance_linear_2.png)

### 확률 벡터의 분산



