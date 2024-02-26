---
layout: single
title: "[AI_theory] 1d Localization - 진행중" 
categories: AI_Theory
typora-root-url: ../
toc: true
use_math: true
---



## Bayesian Filter

실제 베이시안 필터를 이용해서 localization 하는 방법을 정리하고자 한다.  앞선 글에서 정리했듯이 Likelihood와 Prior를 곱한다면, Posterior을 구할 수 있는 것을 확인했습니다. 

<center>$
P(class|data) \approx P(data|class)\times P(class)
$</center>



### Word Coordinate

---

우선, 1 차원 세상을 정의하자. 0부터 100까지 있는 1차원 세상에서 장애물은 80에 위치해 있다고 가정해보자.  아래 그림에서 검은색 = 장애물, 빨간색 = Robot 위치 이다. 

```python
'''
Map Generation
x_map : 1D array
x_map shape : (1, x_length)
value : 0 or 1
'''
x_length = 100
x_land = 80
x_true = 20

x_map = np.zeros((1, x_length), dtype=bool)
x_map[0, x_land] = True


```



![1d_local](/images/2024-02-26-AI_theory4_1d_localization/1d_local.png)



### Mesurement modeling

---

우리가 정의한 로봇은 거리센서를 가지고 있다. 이 센서는 정면으로 100m까지 인지할 수 있으며, 오차 모델링은 평균 0, 표준편차 10을 가지는 센서라고 정의하자. 

위와 같이 정의한다면, 센서로부터 얻는 위치값의 확률분포는 아래와 같을 것이다.  로봇과 장애물의 사이가 60m 이기에,  센서값은 평균 60에, 기존 센서 자체가 가지는 에러이자 표준편차 10을 가진 PDF 그래프가 그려질 것이다. 

```python
z_bin = np.arange(0, 100) 
z_sigma = 1e1
P_measure = norm.pdf(z_bin, x_land-x_true, z_sigma)
```

![1d_local_1](/images/2024-02-26-AI_theory4_1d_localization/1d_local_1.png)

### Likelihood

---

Landmark 위치가 80m에 존재하고 센서로부터 얻은 측정값은 60m로 얻었다라고 가정하자. 

그렇다면 로봇의 위치는 20m로 추정할 수 있고, 센서의 오차를 표준편차로 갖는 가우시안 분포를 갖는 Likelihood 함수를 얻을 수 있다. 

```python
p_likelihood = norm.pdf(x_bin, x_land - z_measure, z_sigma)
p_likelihood = p_likelihood / np.sum(p_likelihood)
```



![1d_local_ideal_likelihood](/images/2024-02-26-AI_theory4_1d_localization/1d_local_ideal_likelihood-1708927156510-8.png)



### Prior

---

Prior 확률은 로봇이 Word coordinate에서 어디에 존재하는지 확률이다. 가장 처음에는 어떠한 단서도 없기 때문에, 균등하게 존재한다고 가정한다. 

예를들어서 word coodinate를 0부터 100까지로 정의했기 때문에 균등분포함수라면 아래와 같은 확률을 갖는다. 

- 0에 로봇이 존재할 확률 : 0.01

- 1에 로봇이 존재할 확률 : 0.01

  .

  .

![1d_local_2](/images/2024-02-26-AI_theory4_1d_localization/1d_local_2.png)





### Posterior

---

위에서 정의한 Likelihood와 Prior를 곱해서 Posterior를 구할 수 있다. 여기서 중요한 것은 재귀적으로 다음 스텝에서는 Posterior 값을 Prior 값으로 치환하면서 다시 Posterior를 구할 수 있게 된다. 

```python
p_posterior = p_likelihood * p_prior
p_posterior = p_posterior / np.sum(p_posterior)
x_estimate = x_bin[np.argmax(p_posterior)]  ## np.argmax -> 배열의 최대값의 인덱스를 반환, p_posterior의 최대값을 가지는 x_bin의 값
p_prior = p_posterior ## 재

```



## Result

위 과정을 재귀적으로 반복한다면  다음과 같다.

### Iteration : 0

센서로부터 얻은 값으로 추정한 Likelihood와 사전에 정의한 Prior

![iteration_1](/images/2024-02-26-AI_theory4_1d_localization/iteration_1.png)

Likelihood와 Prior를 곱한 Posterior

![iteration_2](/images/2024-02-26-AI_theory4_1d_localization/iteration_2.png)

### Iteration : 1

센서로부터 얻은 값으로 추정한 Likelihood와 Iteration 0번째에서 얻은 Posterior를 Prior로 정의

![iteration_3](/images/2024-02-26-AI_theory4_1d_localization/iteration_3.png)

Likelihood와 Prior를 곱한 Posterior

![iteration_4](/images/2024-02-26-AI_theory4_1d_localization/iteration_4.png)

### Iteration : 2

센서로부터 얻은 값으로 추정한 Likelihood와 Iteration 1번째에서 얻은 Posterior를 Prior로 정의

![iteration_5](/images/2024-02-26-AI_theory4_1d_localization/iteration_5.png)

Likelihood와 Prior를 곱한 Posterior

![iteration_6](/images/2024-02-26-AI_theory4_1d_localization/iteration_6.png)
