---
layout: single
title: "[Math_theory] 내적 - 진행중" 
categories: Math_Theory
typora-root-url: ../
toc: true
use_math: true
---





## 내적

내적의 의미는 두 벡터 간 얼마나 비슷한 방향을 향하고 있는지를 나타낸다. 

두 벡터가 수직이라면, 내적은 0

두 벡터가 같은 방향이라면,  내적은 양수

두 벡터가 반대 방향이라면, 내적은 음수



### Scalar Product or Dot Product

---

열 벡터를 가지고 있을 때,  내적은 아래와 같이 구할 수 있다. 

![dotproduct](/images/2024-02-26-Math_theory1_dot_product/dotproduct-1708936684335-21.png)

또한, 이를 ''제2코사인 법칙''에 의해서 아래와 같이 정의가 됩니다. 

![dotproduct2](/images/2024-02-26-Math_theory1_dot_product/dotproduct2.png)

이는 아래와 같이 반대 성분에 정사영(Projection) 한 후, 해당 크기를 곱한 것이라 볼 수 있다.  예를들어, <span style="color:red">A 벡터를 B 벡터에 Projection한 값</span>에 B 벡터의 크기를 곱한 값으로 정의 할 수 있다. 

![dotproduct3](/images/2024-02-26-Math_theory1_dot_product/dotproduct3.png)



### Magnitude of a vector

---

벡터의 크기는 피타고라스 공식을 이용해서 벡터의 길이를 구하면 된다. 그러나 단위 벡터,  즉 Unit vector의 개념을 이해하려면 벡터의 크기가 아래와 같이 계산 되는 것도 알고 있어야 한다. 

![dotproduct4](/images/2024-02-26-Math_theory1_dot_product/dotproduct4.png)

### Unit Vector

---

크기가 1인 벡터를 의미한다. 해당 벡터의 unit vector를 계산하기 위해서는 해당 벡터에 크기를 나눠주면 크기가 1이고 방향이 같은 벡터가 된다. 

예를들어, A 벡터에 A 벡터 크기를 나눠주면, A벡터와 방향은 같고 크기가 1인 Unit Vector가 된다. 이를 Nomalize라고 한다. 

![dotproduct5](/images/2024-02-26-Math_theory1_dot_product/dotproduct5-1708940987264-27.png)

### 정사영 된 좌표값

---

내적으로 정사영 된 벡터의 크기를 구하기 위해서는, 빨간색으로 정의된 공식을 이용해 크기를 구한다. 이것은 단순히 B 벡터의 크기를 나눠준 것이고, B 벡터의 크기 표기 법을 위에서 정리한 식으로 표현했을 뿐이다. 

<span style="color:red">정사영 된 벡터의 크기는 벡터가 아니기에</span>, <span style="color:blue">B 벡터의 단위 벡터 곱해줘야지 파란색으로 그려진 벡터를 표현할 수 있다.</span> 



![dotproduct6](/images/2024-02-26-Math_theory1_dot_product/dotproduct6.png)
