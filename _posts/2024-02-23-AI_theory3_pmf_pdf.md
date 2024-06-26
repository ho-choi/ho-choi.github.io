---
layout: single
title: "[AI_theory] PMF, PDF" 
categories: AI_Theory
typora-root-url: ../
toc: true
use_math: true
---

## PMF

PMF, Probability Mass Function의 사전적인 정의는 다음과 같다

> 이산 확률 변수에 대한 "확률을 나타내는 함수"

이산 확률 변수를 설명하기 위해 특별한 주사위라고 가정해보자. 주사위는 1~6까지 나오는 주사위가 아닌 0~3까지 나오는 주사위이다. 또한 나올 확률들이 균등하지 않고 아래와 같이 다르다고 가정한다. 

![image-20240223173226186](/images/2024-02-23-AI_Theory3/image-20240223173226186.png)

이때 이산 확률 변수란, 특정 값들이 확률적으로 발생할 경우 특정 값들을 이산 확률 변수라고 한다. 예제에서는 주사위를 던져서 발생할 수 있는 수 : 0, 1, 2, 3 이 이산 확률 변수가 되겠다. 

이러한 이산 확률 변수를 함수로 나타낸 것이 PMF 이다. x축이 이산 확률 변수, y축이 확률 값이다. 

![image-20240223172807090](/images/2024-02-23-AI_Theory3/image-20240223172807090.png)



## PDF

PDF, Probability Density Function의 사전적인 정의는 다음과 같다. 

> 연속 확률 변수에 대한 "확률을 나타내는 함수"

연속 확률 변수를 설명하기 위해서 대한민국 평균키를 나타내 보자. 평균키의 경우 이산 확률 변수와 같이 160, 170, 180과 같이 이산적(discrete)으로 현할 수 없다. 따라서 이를 표현하기 위해서 Density 개념이 도입된 것이다. 



우선은 Histogram을 그리기 위해서 구간을 나눈다. '150-160' / '160-165' ... 와 같이 구간을 나눈 후 해당하는 구간의 y축을 Density로 정의한다. 

Density의 사전적인 정의는 아래와 같다 . 즉  질량/부피 이다. 

<center>$
Density  = {mass \over volumn}
$</center>
PDF에서 Density는 확률 / 확률변수 구간의 크기 이다. 여기서 '확률 변수 구간의 크기' 라는 표현이 어려울 수 있다. Historam의 간격을 의미한다. 150-160cm의 사람을 구간으로 나눴기 때문에 Hitogram에서 간격은 10을 의미한다. 하지만 연속 확률 변수 이기에 Histogram의 간격을 줄인 것이 곡선 그래프이며 이것을 PDF라고 한다. 



![그림3](/images/2024-02-23-AI_Theory3/그림3.png)



일반적으로 Continuous RV 관점에서는 $P(x) = \frac{1}{\infty}$ 이다. 따라서 CDF의 개념에서 미분을 통해 PDF를 정의한다. 

 ![pdf_calculate](/images/2024-02-23-AI_theory3_pmf_pdf/pdf_calculate.png)

