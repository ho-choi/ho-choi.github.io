---
layout: single
title: "[Robot_theory] Rotation Matrix" 
categories: Robot_Theory
typora-root-url: ../
toc: true
use_math: true
---





## 2D Rotation Matix

아래 그림에서와 같이 점 p점이 존재할 때, XY Frame 관점에서 보는 p위치와 X'Y' Frame 관점에서 p'위치는 다른 것을 알 수 있다.

하지만 Frame이 $\theta$ 만큼 이동한 것이지,  점P 가 이동한 것은 아니다.



![rotation matrix](/images/2024-03-19-Robot_theory3_Rotation Matrix/rotation matrix-1711334093354-2.png){: .img-width-70 .align-center}

 

따라서, 같은 점 p에 관해서 Frame이 $\theta$ 만큼 이동했을 시 Rotation matrix는 아래와 같다. 





![rotation matrix_1](/images/2024-03-19-Robot_theory3_Rotation Matrix/rotation matrix_1.png){: .img-width-half .align-center}





## 2D Translation Matrix

Translation Matrix는 아래 그림처럼 직관적으로 이해하기 쉽다.

X 축 방향으로 $a$ 만큼 이동한다면 단순히 좌표는 $x+a$ 가 되고, Y축으로 $b$ 만큼 이동한다면 좌표는 $y+b$가 된다. 



![rotation matrix_2](/images/2024-03-19-Robot_theory3_Rotation Matrix/rotation matrix_2-1711335573703-9.png){: .img-width-half .align-center}

## 2D Homogeneous Transform

위에서 Rotation Matrix와 Traslation Matrix 를 살펴보았다. 

실제 로봇이 이동하는 원리는 회전을 하고 평행이동을 하는 것이다. 그것을 수식으로 나타내면 아래와 같다. 



![rotation matrix_3](/images/2024-03-19-Robot_theory3_Rotation Matrix/rotation matrix_3-1711339871250-12.png){: .img-width-30 .align-center}







하지만, 이처럼 Rotation과 Translation을 나눠서 계산하게 된다면 연산량 관점에서 효율적이지 못하다.

이를 위해서,  Rotation과 Translation을 한번에 할 수 있는 Homogeneous Transform 방법을 만들었다.  



![rotation matrix_4](/images/2024-03-19-Robot_theory3_Rotation Matrix/rotation matrix_4.png){: .img-width-30 .align-center}
