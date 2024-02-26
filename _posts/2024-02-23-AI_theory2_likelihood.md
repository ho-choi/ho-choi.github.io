---
layout: single
title: "[AI_theory] Likelihood" 
categories: AI_Theory
typora-root-url: ../
toc: true
use_math: true
---

## Likelihood

Likelihood 개념을 알기 위해서는 Probability Density Function (PDF)에 대해서 먼저 알아야 한다. [PDF개념정리](https://ho-choi.github.io/ai_theory/AI_Theory3/)

베이즈 정리를 보면 Likelihood 수식은 아래와 같다. 

<center>
$
P(Data|Class)
$
</center>

<center>
or
</center>

<center>
$
P(Evidence|Hypothesis)
$
</center>


수식에 대한 표현만 놓고 보면 Class를 알고 있을 때 data의 함수 값이다. 또는, Hypothesis를 알고 있을 때, Evidence의 값이다.  우리가 Class / Hypothesis를 알고 있다고 가정하는 것은 분포의 평균값과 표준편차를 아는 것이라고 정의한다. 

예를들어서, 확률변수로는 대한민국 남자 키 150부터 200 이라고 정의하자. 그리고 대한민국 남성 키의 평균은 175, 표준편차는 10이라고 정의해서 그래프를 그리면 아래와 같다. 

```python
x_variable = np.arange(150,200,1)
x_mean = 175
x_sigma = 10

p_measure = norm.pdf(x_variable, x_mean, x_sigma)
```

![output](/images/2024-02-23-AI_Theory2/output.png)



이때, 확률을 구하기 위해서는 특정구간을 정의해서 적분을 해야 계산된다. 

예를들어 대한민국에서 남자 한명을 랜덤으로 뽑았을 때, 160~170cm 남자를 뽑을 확률은 아래 빨간색으로 정의된 면적이다. 

![output2](/images/2024-02-23-AI_Theory2/output2.png)



우리가 원하는 것이 170cm 남자를 뽑을 확률을 알고 싶다고 했을 때, 아래 그래프와 같이 0.035가 확률이 아니다. 왜냐하면 y축은 확률밀도(Probability Density)이다. 그래서 이 값을 확률이라 부르지 않고, Likelihood 라고 정의한 것이다. 

![output1](/images/2024-02-23-AI_Theory2/output1.png)
