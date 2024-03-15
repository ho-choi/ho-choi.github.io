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


수식에 대한 표현만 놓고 보면 Class를 알고 있을 때 data의 함수 값이다. 또는, Hypothesis를 알고 있을 때, Evidence의 함수값이다. PDF 에서 함수값이라는 것은 확률이 아니기에, 이를  Likelihood 라고 정의한 것이다.

Class에 따른 Data 분포의 평균값과 표준편차를 안다라고 가정하면, 어떠한 데이터가 들어오더라도 Likelihood를 계산할 수 있다. 

 조금 더 구체적으로 말해보면 Class_1 = 대한민국, Class_2 =  벨기에, 라고 가정해보자. 대한민국과 벨기에 사람 중 무작위로 한명을 뽑았을 경우, 이사람이 Class1 일지 Class2 일지 어떻게 구별하면 좋을까? 신장, 즉 키는 Class 를 구분할 수 있는 요인중에 하나일 것이다.  대한민국 평균키가 175이고, 벨기에 키가 185라고 가정하고, 무작위로 뽑은 한사람의 키가 170이라고 가정한다면 이사람의 국가는 대한민국일 Likelihood가 높기 때문이다. 



![Likelihood bel_korea](/images/2024-02-23-AI_theory2_likelihood/Likelihood bel_korea.png)



이해가 안간다면, 하나하나 그래프를 그려가며 이해해보자.

예를들어서, 확률변수로는 대한민국 남자 키 150부터 200 이라고 정의하자. 그리고 대한민국 남성 키의 평균은 175, 표준편차는 10이라고 정의해서 그래프를 그리면 아래와 같다. 

```python
x_variable = np.arange(150,200,1)
x_mean = 175
x_sigma = 10

p_measure = norm.pdf(x_variable, x_mean, x_sigma)
```



![Likelihood korea_2](/images/2024-02-23-AI_theory2_likelihood/Likelihood korea_2.png)

이때, 확률을 구하기 위해서는 특정구간을 정의해서 적분을 해야 계산된다. 

예를들어 대한민국에서 남자 한명을 랜덤으로 뽑았을 때, 160~170cm 남자를 뽑을 확률은 아래 빨간색으로 정의된 면적이다. 



![likelihood_korea_probability](/images/2024-02-23-AI_theory2_likelihood/likelihood_korea_probability.png)



우리가 원하는 것이 170cm 남자를 뽑을 확률을 알고 싶다고 했을 때, 아래 그래프와 같이 0.035가 확률이 아니다. 왜냐하면 y축은 확률밀도(Probability Density)이다. 그래서 이 값을 확률이라 부르지 않고, Likelihood 라고 정의한 것이다.  위에서 설명했을 때, Data의 함수값이라고 표현했던 이유이다. 



![Likelihood korea](/images/2024-02-23-AI_theory2_likelihood/Likelihood korea-1710477542933-10.png)

이제는 벨기에 남자들의 키 분포를 그리면 아래와 같다. 아래와 같이 170cm의 Likelihood를 구하면 아래와 같이 구할 수 있다. 이때 Likelihood 값은 0.012값을 얻을 수 있다. 



![Likelihood bel_gelgium](/images/2024-02-23-AI_theory2_likelihood/Likelihood bel_gelgium.png)



비교하기 쉽게 한국과 벨기에의 PDF를  비교해보면 Likelihood를 더 쉽게 비교할 수 있다.  

이제 정리해보면, 우리가 사전에 한국과 벨기에의 평균키를 알고 있다면, 무작위로 사람을 한명 뽑았을 경우 이 사람이 한국사람일 가능도가 벨기에 사람일 가능도보다 더 높다는 것을 알 수 있다.



![Likelihood bel_korea](/images/2024-02-23-AI_theory2_likelihood/Likelihood bel_korea-1710477780918-12.png)



### Likelihood - Robot Localization

우리는 로봇 어플리케이션에서 Localization이라는 단어를 많이 듣게 된다. 이때, 베이지안 필터를 이용해서 Localization을 진행하게 되는데, 여기서 나오는 Likelihood에 대해서 정리해 보자.

Likelihood 식을 보면 아래와 같다. 수식만 놓고보면 x 를 알고 있을 때, z의 확률이다.  

<center>$
 P(z|x)
$</center>

- x=1 일 때, 얻을 수 있었던 z의 확률 

- x=2 일 때, 얻을 수 있었던 z의 확률
- ...



위와 같이 식만 놓고 보면 Localization과 Likelihood가 어떻게 상관관계가 있는지 이해하기 어렵다. 이를 이해하기 쉽게 Likelihood를 사전데이터를 수집해서 데이터베이스를 구축한다는 관점으로 생각해 보자.  

실제 로봇은 여러 센서들로부터 거리, 각도, Point Cloud 등 다양한 z값이 추가될 수 있다고 가정해보자.

아래 표는 로봇이 존재할 수 있는 모든 위치(Class)에 대해서 센서 데이터를 모아놓고, 각 센서 데이터 별 평균과 분산을 저장해 놓은 데이터베이스 테이블이다. 

예를들어 로봇의 위치가 x=1 일 때, 거리측정 센서로부터 얻을 수 있는 Diatance 값들을 모아서 평균과 분산을 구해 데이터베이스화 시켜놓은 것이다. 

![Likelihood Table](/images/2024-02-23-AI_theory2_likelihood/Likelihood Table.png)

사실, 어떤 센서 값의 평균과 분산값을 알고 있다는 것은 Class의 특징을 알고 있다는 것과 같은 말이다. 위에서 예를 든 것처럼 대한민국과 벨기에 남성 키의 평균과 분산을 알고있을 경우를 생각하면 쉽다.



