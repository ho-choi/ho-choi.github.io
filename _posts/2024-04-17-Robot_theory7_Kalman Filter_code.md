---
layout: single
title: "[Robot_theory] Kalman Filter_code" 
categories: Robot_Theory
typora-root-url: ../
toc: true
use_math: true
---



## 1D Kalman Filter

1차원 세상에 로봇이 있다고 하자. 즉, 로봇의 위치를 x 축 위에 있다고 생각하면 된다. 이 로봇의 위치는 아래와 같이 움직인다고 모델링을 해보자 
$$
\bar{x}_k = x_{k-1} + v_{k}\cdot\Delta t
$$
아래 Class에서 move() 메소드에 코드로 작성되어 있다. 

속도는 process_var이라는 오차가 존재한다고 가정한다. 

또한 sense_position() 메소드 에서는 로봇의 위치를 측정하는 역할이고, 현재 로봇위치에서 meas_std라는 측정오차를 더했다. 

현재 위치가 process_var 오차가 더해져 있는 상태에서 한번 더 meas_std 오차가 더해지기 때문에, 측정오차가 더 크다고 할 수 있다.

```python

class RobotSimulation(object):
    def __init__(self, x0=0, velocity=1,
                 measurement_var=0.0,
                 process_var=0.0):
        """ x0 : initial position
            velocity: (+=right, -=left)
            measurement_var: variance in measurement m^2
            process_var: variance in process (m/s)^2
        """
        self.x = x0
        self.velocity = velocity
        self.meas_std = sqrt(measurement_var)
        self.process_std = sqrt(process_var)

    def move(self, dt=1.0):
        """Compute new position of the dog in dt seconds."""
        dx = self.velocity + randn()*self.process_std
        self.x += dx * dt

    def sense_position(self):
        """ Returns measurement of new position in meters."""
        measurement = self.x + randn()*self.meas_std
        return measurement

    def move_and_sense(self):
        """ Move dog, and return measurement of new position in meters"""
        self.move()
        return self.sense_position()

```



### Bayesian Filter

---

칼만 필터는 베이지안 필터의 Likelihood, Prior를 가우시안 분포라고 가정하는 것과 같다.  베이지안 필터는 아래와 같이 Posterior를 근사화 할 수 있다. 이해가 안된다면 참고  [Bayesian Filter](https://ho-choi.github.io/ai_theory/AI_theory1_posterior/)
$$
Posterior \approx Likelihood \times Prior
$$

- Prior : 이전 스텝 K-1 시점에 상태 변수를 가지고 K 시점을 예측한 값이다. 즉 칼만 필터에서 Prediction 내용에 해당된다.
- Likelihood : 센서로부터 얻은 측정값이다. 센서값 역시 가우시안 분포를 갖는다. 
- Posterior : Prior 와 Likelihood 두 가우시안 분포의 곱이다. 이를 K 시점의 Prior로 대체하는 것이 칼만 필터에서 Update 과정이다.



#### Product of Gaussian PDF

두개의 가우시안 분포를 곱했을 경우, 하나의 가우시안 분포가 나오게 된다. 이때 결과값으로 나온 가우시안 분포는 아래와 같은 평균과 분산을 갖는다. 
$$
\mu = \frac{\sigma^{2}_1\mu_2+\sigma^{2}_2\mu_1}{\sigma^{2}_1+\sigma^{2}_2}
$$

$$
\sigma^{2} = \frac{\sigma^{2}_1\sigma^{2}_2}{\sigma^{2}_1+\sigma^{2}_2}
$$

위와 같은 식이 나오는 증명 과정은 아래 그림을 참고하면 된다.

우리는 Prior를 예측값이라고 했다. 따라서 Prior의 가우시안 분포는 아래와 같다. 
$$
N(\bar{\mu}, \bar{\sigma}^2)
$$


또한,  Likelihood는 측정값을 가지고 추정한 로봇의 위치 이기 때문에 가우시안 분포는 아래와 같이 표기한다. 
$$
N(\mu_z, \sigma^2_z)
$$


Prior와 Likelihood의 곱으로 나타내는 Posterior는 아래와 같다. 
$$
N(\frac{\bar{\sigma}^2\mu_z + {\sigma_z}^{2}\bar{\mu}}{\bar{\sigma}^{2}+\sigma^{2}_z}, \frac{\bar{\sigma}^{2}\sigma^{2}_z}{\bar{\sigma}^{2}+\sigma^{2}_z})
$$


위와 같은 수식을 코드로 변환하면 아래와 같다.

```python
def gaussian_multiply(g1, g2):
    mean = (g1.var * g2.mean + g2.var * g1.mean) / (g1.var + g2.var)
    variance = (g1.var * g2.var) / (g1.var + g2.var)
    return gaussian(mean, variance)

def update(prior, likelihood):
    posterior = gaussian_multiply(likelihood, prior)
    return posterior

# test the update function
predicted_pos = gaussian(10., .2**2)
measured_pos = gaussian(11., .1**2)
estimated_pos = update(predicted_pos, measured_pos)
estimated_pos
```









![kf_9](/images/2024-04-17-Robot_theory7_Kalman Filter_code/kf_9.png)



