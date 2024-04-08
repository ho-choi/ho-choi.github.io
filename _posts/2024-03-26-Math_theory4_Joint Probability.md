---
layout: single
title: "[Math_theory] Joint Probability" 
categories: Math_Theory
typora-root-url: ../
toc: true
use_math: true
---

## Joint Probability



### Joint CDF

cumulative distribution function, CDF 의 정의는 아래와 같다. 확률변수가 특정 값보다 작거나 같은 확률을 나타내는 함수이다

![joint_probability_3](/images/2024-03-26-Math_theory4_Joint Probability/joint_probability_3.png){: .img-width-30 .align-center}



Joint probability 관점에서 CDF에 대한 정의는 아래와 같다. 

![joint_probability](/images/2024-03-26-Math_theory4_Joint Probability/joint_probability.png){: .img-width-30 .align-center}

### Joint PDF

PDF 는 CDF의 미분이라고 아래 페이지에서 정의를 하였다. [PDF정의](https://ho-choi.github.io/ai_theory/AI_theory3_pmf_pdf/)

Joint PDF 역시 위에서 정의한 Joint CDF의 이중미분과 같다. 

이를 반대로 말하면, PDF의 이중적분이 CDF라고 생각할 수 있다. 



![joint_probability_2](/images/2024-03-26-Math_theory4_Joint Probability/joint_probability_2-1711502329385-1.png){: .img-width-30 .align-center}





## Maginal Probability

다른 사건을 고려하지 않고 특정 사건이 발생할 확률을 나타낸다. 이는 다른 변수의 가능한 모든 값을 합산하거나 통합하여 결합 확률 분포에서 파생된다.

예를들어서, $P(X, Y)$ 라는 joint probability가 존재할 때, 내가 관심있는 확률은 X가 일어날 확률이라고 정의해보자. 

그럴때는 Y가 일어날 확률에 대해서 모두 더해준다면 X가 일어날 확률인 것이다. 



![joint_probability_5](/images/2024-03-26-Math_theory4_Joint Probability/joint_probability_5.png){: .img-width-30 .align-center}





![image-20240327105115395](/images/2024-03-26-Math_theory4_Joint Probability/image-20240327105115395.png){: .img-width-30 .align-center}

