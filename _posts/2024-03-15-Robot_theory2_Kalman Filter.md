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
