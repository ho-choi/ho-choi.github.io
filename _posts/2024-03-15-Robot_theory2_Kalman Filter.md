---
layout: single
title: "[Robot_theory] Kalman Filter" 
categories: Robot_Theory
typora-root-url: ../
toc: true
use_math: true
---

## Kalman Filter

Markov Localization은 로봇 위치에 대한 Likelihood와 이전 스텝에서 얻은 Prior를 곱하면서 Posterior를  Update하는 과정이었다. [Markov Localization](https://ho-choi.github.io/ai_theory/AI_theory4_1d_localization/) 

Kalman Filter 역시 같은 개념이다. 여기서 Markov Localization과 다른 것은 아래와 같다.

- Likelihood : Linear Model을 이용해서 위치 추정 - (Kalman Filter에서는 Prediction이라 불린다.)
- Update : Kalman Gain을 구해서 Prediction된 값과 센서로부터 얻은 값을 융합해서 State Variable을 Update



직관적인 설명을 위해, 로봇의 위치를 추정하는 Kalman Filter의  Pseudo-Code와  결과값을 먼저 살펴보자.

```python
0. initialize
    x_position ## 로봇의 위치를 초기에는 설정을 해줘야한다. 
    P		   ## 로봇의 위치에 대한 초기 covariance 설정한다.

for t in range(1, 2): ## 센서값이 들어오는 주기 또는 Update 하는 횟수
	
    1. Correct
        1-1. Measurement using sensor
        |	Z = C @ x_position + delta      ## 센서는 로봇의 위치를 산출해주는 센서라 가정한다면, C는 Identity Matrix가 된다.
        |						   		    ## delta는 센서의 오차이다.
        |	

        1-2. Compute the Kalman Gain             ## Kalman Gain을 계산 
        |	K = P @ H.T @ ((H@P@H.T)+Q)^1	## 수식은 그저 kalman gain구하는 공식을 코드화 시켜놓은 것 뿐이다.

        1-3. Update x_position using sensor      ## 로봇의 위치 업데이트, 로봇의 위치 에러 Covariance 업데이트
        |	new_position = x_position @ K @ (Z-(H @ x_position))
        |	new_P = (I - K@H)@P 

	2. Control
    	Controll value : U
        
    3. Predict
    	x_position = A @ new_position + B @ U  ## Correct를 통해 얻은 정보를 가지고 로봇의 위치를 Predict
        P		   = A @ new_P @ A.T + R	   ## Correct를 통해 얻은 정보를 가지고 위치에 대한 Covariance Predict
        
    4. Return 1
```




센서값을 한번 받는다면 아래와 같은 결과가 나타난다. 

Pseudo Code

0번 : 빨간색은 로봇의 초기 위치값으로 설정한 (0,0)으로 셋팅 되어 있고, Covariance는 random으로 발생시켜 원을 생성하였다.

​	1-1번 : 센서로부터 로봇의 위치값을 수신하였다. 그래프 상에서 하늘색 x

​	1-2번 : Kalman Gain을 계산한다.

​	1-3번: 이전 위치값과 센서 측정값을 통해  update 한 위치는 파란색 원의 중점, Covariance는 원의 크기 이다.

2번 :  로봇의 제어값을 입력 받는다. 

3번 :  1번을 통해 update된 위치값을 기반으로 제어값을 도입해서 로봇의 위치를 예측한다.

![kf_1](/images/2024-03-15-Robot_theory2_Kalman Filter/kf_1.png)

위와 같은 과정을 10번 loop를 돌았다고 가정하면 아래와 같은 결과를 얻을 수 있다.

![kf_2](/images/2024-03-15-Robot_theory2_Kalman Filter/kf_2.png)





### State

X를 하나의 값이 아닌, 평균과 분포를 가지고 있다.  라는 관점의 변환을 가지고 온 것이 칼만필터의 시작이다. 



### Prediction

![kf_3](/images/2024-03-15-Robot_theory2_Kalman Filter/kf_3.png){: .img-width-70 .align-center}

<br>

위 그림이 Kalman Filter의 전체적인 그림이다.  복잡해 보이지만, 가장 중요한 것은 Prediction은 k-1 에서 k로 공분산과 state 변수가 정의 된다는 것이다. 

$X$는 State Variable로써, 시스템 모델링에 따라 달라질 수 있다. 칼만필터 이전에는 $X$를 하나의 값으로 정의해오고, 시스템 모델링을 통해 값을 예측해 나갔다면, 칼만필터는  $X$가 하나의 값이 아닌 $x_hat$  이라는 평균값과 P라는 공분산을 가지고 있다고 가정한다. 

아래 그림에서 왼쪽이 기존 방식, 오른쪽이 칼만필터 방식이라고 생각하면 된다. 



![kf_4](/images/2024-03-15-Robot_theory2_Kalman Filter/kf_4.png)

<br>

분산은 각 데이터가 평균에서 얼마나 멀리 떨어져 있는지를 나타낸다.  분산 정의 식은 아래와 같다 .

![kf_5](/images/2024-03-15-Robot_theory2_Kalman Filter/kf_5.png){: .img-width-30 .align-center}

<br>

우리는 $X$ 를 상태변수로 정의했기 때문에 여러개의 벡터로 이뤄져 있다. 예를들어 상태변수를 좌표로 정의했을 경우, $X = {x, y}$ 로 정의할 수 있다. 

따라서, 벡터에 대한 공분산은 아래와 같이 정의할 수 있다. 



![kf_6](/images/2024-03-15-Robot_theory2_Kalman Filter/kf_6-1713235240085-4.png){: .img-width-30 .align-center}

<br>

우리가 계산해야 할 것은 $P_{k+1}$` 이다.  여기서 위에 '  이 붙는 것은 prediction을 의미한다. 

![kf_7](/images/2024-03-15-Robot_theory2_Kalman Filter/kf_7-1713237156909-7.png)

<br>





Kalmal Gain은 공분산을 작게 해주는 K를 구함으로 써, x와 $x_hat$이 최대한 같도록 만들어 주는 것이 kalman Filter  이다. 

KF에서는 K-1이 현재고 K가 다음 스텝을 의미한다. 



반대로 Update step에서는 k에서 k 로 이뤄져 있다는 것을 유심히 보면 된다. 



크게 4가지 block으로 이뤄져 있다. 

#### State Equation

#### Prediction

위에  상태방정식을 보면 오차가 포함되지만, 오차는 예측이 불가능하기 때문에 예측에서는  X+U로 이뤄져있다. 

#### Kalman Gain

위 Prediction 과정을 계속해서 진행한다면 $Q$ 때문에 계속해서 오차만 커지게 된다.  이를 위해서 관측된 값을 이용해서 Prediction을 통해 얻은 값을 보정하는 요소가 필요하다. 이것이 Kalman Gain이다. 



우리가 궁극적으로 알고 싶은것은 Estimation, 즉 추정값 $\hat{X}_{k+1}$ 을 구하는 것이다.  

이를 구하기 위해서 $\hat{X}^\prime_{k+1}$ 값을 Prediction하고, H와 곱을 통해 $\hat{Z}^\prime_{k+1}$ 을 prediction 한다. 

실제 $t+1$ 일 때, 측정한 $Z_{k+1}$ 값과 Prediction 한 $\hat{Z}^\prime_{k+1}$ 의 차이와, 

Estimation 한 $\hat{X}_{k+1}$ 와 Predition 한 ${\hat{X}^\prime}_k+1$ 의 Gain을 구하는 것이다. 



<br>

![kf_8](/images/2024-03-15-Robot_theory2_Kalman Filter/kf_8-1713241386374-10.png)







#### Correction = Update

우리는 Estimation의 P, 즉 $P_{k}$를 최소화 시키는 것이 목표이다. Prediction  하고 Kalman Gain을 구하는 것도 모두 $P_{k}$를 최소화 시키기 위해 구하는 것이다.   최소화 문제는 Objective Function을 정의하고 이를 미분하는 것이 방법을 취한다. 

따라서 우리는 Objective Function은 $P_{k}$로 정의하고 이는 아래와 같다. 



우리가 하고 싶은것은, prediction 한 가우시안 분포의 공분산 P를 줄이는 것이 목표이다. 





## Gaunssian mean / variable



![image-20240417145034296](/images/2024-03-15-Robot_theory2_Kalman Filter/image-20240417145034296.png)

