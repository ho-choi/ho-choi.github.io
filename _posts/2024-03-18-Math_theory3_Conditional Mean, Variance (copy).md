---
layout: single
title: "[Math_theory] 조건부 평균과 조건부분산" 
categories: Math_Theory
typora-root-url: ../
toc: true
use_math: true
---



## 조건부 평균

평균과 분산 글에서 봤듯이, 확률변수가 이산이냐 연속이냐에 따라서 평균과 분산을 구하는 방법들이 달랐다. [평균과 분산](https://ho-choi.github.io/math_theory/Math_theory2_Mean,-Variance/) 

연속확률변수의 평균을 구하는 식에서 조건부만 기술해 주면 된다.  

조건부 확률밀도함수, 즉 Conditional PDF는 아래와 같이 정리할 수 있다. #Conditional_PDF 참고

![Expectation_conditional](/images/2024-03-18-Math_theory3_Conditional Mean, Variance (copy)/Expectation_conditional-1710745256163-3.png)



$x$ 에 대해서 적분하기 때문에 평균에 관한 식은 $y$에 관한 식으로 나올 것이다.  즉, $f(y)$ 형태일 것이다.



### Conditional PDF

PDF의 개념은 CDF를 미분한 값이라고 정리했었다. [PDF](https://ho-choi.github.io/ai_theory/AI_theory3_pmf_pdf/)

조건부 PDF 역시 같은 개념으로 정리가 되는데, 다를 것은 없고 뒤에 조건부만 붙여서 생각해주면 된다.

1.  $x$에 대한 PDF를 구하는 것이기 때문에 $x$에 대한 미분과 $x$에 대한 CDF로 정리해 준다. 조건부는 뒤에 첨자로 적어주면 된다.

2. CDF에 정의를 보면 $F_x = P(X<x)$ 이기 때문에, 두번째 수식으로 풀어서 쓸 수 있다. 
3. 조건부확률 정리를 통해서 식을 정리한다.
4. Continuous RV 의 경우 특정 $Y=y$ 값을 구하기 힘들기 때문에 $dy$ 값을 도입해서 식을 정리한다. 
5. CDF 개념을 도입해서 식을 정리한다. (그림 참고하면 이해하기 쉽다.)
6. 미분을 도입하기 위해서 $dy$를 나눠주면서 정리해준다.



![conditional_pdf](/images/2024-03-18-Math_theory3_Conditional Mean, Variance (copy)/conditional_pdf.png)





### ## 조건부 분산

조건부 분산 역시, 분산을 구하는 식에 아래와 같이 조건부를 써주면 된다.



![variance_conditional](/images/2024-03-18-Math_theory3_Conditional Mean, Variance (copy)/variance_conditional.png)



