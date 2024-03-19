---
layout: single
title: "[AI_theory] 1d Markov Localization - 진행중" 
categories: AI_Theory
typora-root-url: ../
toc: true
use_math: true
---




## 1D Markov Localization

Markov Localization이란, Bayesian 필터를 이용해서 현재 위치를 추정하는 방법이다. 

아래식을 보면, x는 로봇 위치의 확률이고,  z는 센서로부터 얻은 센서값이다.  우리가 궁극적으로 원하는 건 Posterior 값이다. 즉, 센서값을 얻었을 때 로봇이 실제 존재하는 위치의 확률을 극대화 하고 싶은 것이다. 

<center>$
P(x|z) \approx P(z|x)\times P(x)
$</center>







예를들어, 실제 로봇이 x = 3 위치에 존재한다고 가정했을 때,  센서값 z = 10 얻었다면 로봇이 위치 확률값은 아래와 나올 것이다. 

- P(x=1|z=10) = 0
- P(x=2|z=10) = 0
- P(x=3|z=10) = 1
- P(x=4|z=10) = 0

그러나 우리는 센서값만 가지고 로봇의 위치를 추정하는 것은 매우 어렵다. 그렇기 때문에 위 식에서 우항인 Likelihood와 Prior를 곱해가면서 Posterior를 추정해 나가는 것이 Markov Localization 방식이다. 




### 환경설정

로봇의 환경은 아래와 같다.

- 로봇은 1차원 0부터 100까지만 존재할 수 있다.  
- 로봇은 오차를 갖는 거리측정 센서를 가지고 있다.
-  로봇은 LandMark의 정확한 위치를 알고 있다. 



위와 같은 환경일 때, 로봇의 초기 위치는 20 이고,  Landmark의 위치는 80 이라고 해보자. 그러면 아래와 같은 환경에 놓이게 된다. 

![1d_local_size](/images/2024-02-26-AI_theory4_1d_localization/1d_local_size.png)

### 센서 모델링

이때, 로봇이 가지고 있는 거리측정 센서를 이용해서 Landmark 얻은 거리값을 얻게 된다면 로봇은 본인의 위치를 계산할 수 있을 것이다. 로봇이 가진 센서는 인지 범위가 100까지 이고, 측정에 대한 분산은 10을 갖는다고 가정하자.

```python
z_bin = np.arange(0, 100) ## 센서 측정 범위 설정
z_sigma = 1e1 ## 센서가 갖는 분산
P_measure = norm.pdf(z_bin, x_land-x_true, z_sigma)  ## 랜드마크와 로봇 간 거리를 측정한 확률분포
```



예를들어 로봇이 자기의 위치를 모른다고 가정해보자. 그 상태에서 센서로부터 얻은 값이 60 이라면, 랜드마크 위치를 알고있는 로봇 입장에서 본인의 위치가 20일 것이라고 추정할 수 있을 것이다. 그러나 실제 세상에서는 센서로부터 얻는 값은 오차가 존재하기 마련이다. 센서값을 지속적으로 받아서 평균을 내본다면 60이라는 값으로 수렴할 것이다. 또한 오차만큼 분산을 가지게 될텐데 가우시안 분포를 가진다고 가정하면 아래와 같은 그래프가 만들어진다. 





![1d_local_1](/images/2024-02-26-AI_theory4_1d_localization/1d_local_1-1710396181514-2.png)





### Likelihood of Position

로봇은 랜드마크 위치를 알고 있다고 가정했기 때문에 센서값으로부터 로봇의 위치를 추정할 수 있다. 로봇이 자신의 위치를 추정한 과정은 아래와 같다.

- 랜드마크는 0부터 100까지 1차원 지도중에 80 위치에 있는것을 로봇은 알고있다.
- 랜드마크와 로봇은 60만큼 떨어져 있다는 정보를 센서로부터 얻을 수 있다.
- 따라서, 로봇은 20 위치에 존재할 가능성이 높다. 
- 다만, 센서는 오차가 존재하기 때문에 로봇이 존재하는 위치 역시 오차가 존재한다.

![1d_local_5](/images/2024-02-26-AI_theory4_1d_localization/1d_local_5.png)



### Prior

처음 로봇의 위치는 0부터 100까지 위치 중 어디에 있는지 전혀 알 수가 없다. 마치 텅빈 공간에 로봇 혼자 덩그러니 있는 셈이다. 그렇기에 0부터 100까지 로봇이 존재할 확률을 Uniform 하다고 초기 설정을 하는 것이다.



![1d_uniform](/images/2024-02-26-AI_theory4_1d_localization/1d_uniform.png)





### Example - Markov Localization

실제 센서로부터 얻는 값은 정확히 60이 아닌 센서 모델링에서 정의한 가우시안 분포 중 하나의 값을 얻게 된다. 

```python
P_measure = norm.pdf(z_bin, x_land-x_true, z_sigma)  ## 랜드마크와 로봇 간 거리를 측정한 확률분포
z_measure = np.random.choice(z_bin, 1, p=P_measure)  ## 측정 확률분포에서 하나를 뽑아, 이를 센서값이라 정의
```



센서로부터 얻은 값이 53이라고 가정한다면, 로봇은 자신의 위치 추정을 27의 위치에 있을거라고 추정하게 된다. 위치 추정 그래프는 아래와 같다. 

이를 초기 Prior값을 곱하면 변함없는 그래프가 만들어 질것이다.  왜냐하면 Prior를 Uniform으로 정의했기 때문이다.

![1d_local_6](/images/2024-02-26-AI_theory4_1d_localization/1d_local_6.png)



Likelihood와 Prior의 곱을 통해서 Posterior를 얻을 수 있다. 얻은 Posterior를 다음번 스텝에서 Prior로 취해주는 것이 Markov Localization의 개념이다. 

아래 와 같이 10번의 iteration을 돌게 되면 아래와 같이 로봇의 위치 추정 정확도가 높아지는 것을 확인할 수 있다. 

```python
for iteration in range(10):
    ## Probability of measurement
    P_measure = norm.pdf(z_bin, x_land-x_true, z_sigma)
    P_measure = P_measure / np.sum(P_measure)

    ## choice measurement
    z_measure = np.random.choice(z_bin, 1, p=P_measure)

    ## likelihood of position
    p_likelihood = norm.pdf(x_bin, x_land - z_measure, z_sigma)
    p_likelihood = p_likelihood / np.sum(p_likelihood)
    
    ## Posterior Probability
    p_posterior = p_likelihood * p_prior
    p_posterior = p_posterior / np.sum(p_posterior)
    
    ## Prior update
    p_prior = p_posterior    
```



![1d_local_7](/images/2024-02-26-AI_theory4_1d_localization/1d_local_7.png)

