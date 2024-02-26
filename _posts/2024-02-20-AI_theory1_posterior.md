---
layout: single
title: "[AI_theory] Posterior" 
categories: AI_Theory
typora-root-url: ../
toc: true
use_math: true
---



# Posterior
---




인공지능을 공부하다 보면 Posterior, Likelihood, Prior 이라는 용어가 많이 나온다. 아래와 같이 베이즈 정리에서 용어를 정의할 수 있다. 

![image-20240220073649016](/images/2024-02-20-AI_theory/image-20240220073649016.png)





위와 같이 A, B 같은 표현법으로는 더욱 헷갈린다.  아래와 같이 이름을 변경하면 이해하기 쉽다. 

- A 를 Class 로 변경
- B 를 Data  로 변경



$
P(class|data) = {P(data|class)\times P(class) \over P(data)}
$



우리가 실생활에서  Data가 주어졌을 때 Class 를 판단하는 경우가 부지기수다. 하지만 정확하게 판단하기는 쉽지 않다. 예를 들어서, 강아지 사진이 주어졌다고 가정해보자.

![image-20240220082354146](/images/2024-02-20-AI_theory/image-20240220082354146.png)

우리 눈으로는 강아지 사진으로 보이지만, 실제 컴퓨터 입장에서는 Pixel 값으로 이뤄져 있다. 즉, 컴퓨터는 아래 그림과 같이 Pixel 값만 보고 이것이 강아지인지, 고양이인지 구분해야 한다. 

![image](/images/2024-02-20-AI_theory/image.png)



실생활에서는 수많은 변수들이 있기 때문에, Data만 가지고 Class를 구분하기에는 쉽지 않다. 아래 사진만 봤을 때, Pixel 값으로 강아지인지 쿠키인지 구분이 어렵기 때문이다. 

![fake_dog](/images/2024-02-20-AI_theory/fake_dog.png)

이러한 문제를 해결하기 위해서 Likelihood 개념이 도입된다.  위에서 언급했던 베이즈 정리에서 분모를 생략한다면 아래 수식과 같은 근사 관계를 가지게 된다.  

<center>$
P(class|data) \approx P(data|class)\times P(class)
$</center>



> Maximum Likelihood
>
> Posterior 확률을 최대한 높이기 위해서는 Likelihood를 최대화하는 개념



###  Posterior - Robot Localization 

---

예를들기 위해, 두가지 가정을 해보자.

- World Coordinate, 월드 좌표계는 2D Grid map
- 로봇은 Range-Bearing-Sensor 보유

Robot이 알 수 있는 것은 Points 뿐이다. 

<center>$
Point  = {Distance, Angular}
$</center>

예를들어, {distance=10, angular=30} 이라는 값을 얻었을 때, 아래와 같은 2D Grid map에서는 내 위치를 추정하기가 매우 어려운 문제이다. 따라서, Localization 하는 방법에 대해서는 차근차근 포스팅을 해나갈 예정이다. 


![Regular-grid-of-2D-image](/images/2024-02-20-AI_theory/Regular-grid-of-2D-image.png)
