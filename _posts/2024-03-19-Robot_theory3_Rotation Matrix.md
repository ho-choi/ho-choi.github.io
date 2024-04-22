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



## 3D Homogeneous Transform

위에서 살펴본 2D Transform을 3D로 확장시켜 보자. 

아래와 같이,  각축의 Rotation Matrix는 아래와 같이 기술 할 수 있다. 

![rotation matrix_5](/images/2024-03-19-Robot_theory3_Rotation Matrix/rotation matrix_5.png)



여기서, 우리가 주목해야할 부분은, y축 Rotation Matrix는 부호가 반대인 것을 확인할 수 있다. 이는 오른손 법칙을 이용했을 때,  x-y-z 평면에서 y축을 화면 밖으로 올라오는 방향이라고 생각한다면 z-x 평면으로 회전은 마이너스 방향이다. 따라서 Rotation Matrix 부호가 반대가 되는 것이다. 

![rotation matrix_6](/images/2024-03-19-Robot_theory3_Rotation Matrix/rotation matrix_6.png){: .img-width-30 .align-center}

## Forward Kinematics

Forward Kinematics, KF 는 축을 몇도 돌렸을 때, 최종위치는 어디있겠는가 라는 문제이다. 

수식은 아래와 같이 작성할 수 있으며, $\Theta$ 는 벡터로써, 관절 $\theta$ 각의 집합이다.  

$X = FK(\Theta)$



![FK_1](/images/2024-03-19-Robot_theory3_Rotation Matrix/FK_1.png){: .img-width-30 .align-center}

직렬형 로봇은 FK가 쉽고, 병렬형 로봇은 IK가 쉽다고 알려져 있다. 

FK의 경우 아래와 같이 수식으로 직접 $(x, y)$ 를 계산할 수 있지만, $\theta$가 많아질 경우 계산하기 힘들어진다. 

![FK_2](/images/2024-03-19-Robot_theory3_Rotation Matrix/FK_2.png){: .img-width-half .align-center}





이를 해결하기 위해 Homogineous Transform을 이용해서 계산한다. 
아래와 같이 원점 좌표계에서 빨간색 좌표계로,   빨간색 좌표계에서 파란색 좌표계로 이동하는 개념으로 생각하면 된다. 



![FK_3](/images/2024-03-19-Robot_theory3_Rotation Matrix/FK_3-1712571791766-6.png){: .img-width-half .align-center}









## Inverse Kinematics

Inverse Kinematics, IK 는 최종 위치값을 넣으면 관절의 $\theta$ 값을 알아내는 것이다. 







## 쿼터니안



xyz 좌표계에서 어떠한 원점을 지나는 벡터가 존재한다고 가정하고, 임의의 공간을 표현하는 벡터가 있다고 하면 그를 AK라는 벡터라하자. 임의의 백터를 기준으로 돌리겠다고 가정하자. 



돌리는 방햔은 오른손 법치을 따른다. 이 벡터를 중심으로  theta  만큼 돌린다. 

임의의 벡터를 표현하는 방법은  3개의 스칼라 열벡터로 정의한다. 벡터 K를 따라서   theta  만큼 돌리기에 이를