---
layout: single
title: "[SLAM_theory] Visual Odometry - ORB" 
categories: SLAM_Theory
typora-root-url: ../
toc: true
use_math: true
---

## Visual Odometry

SLAM 시스템은 FrontEnd / Backend 로 나뉘고, Front-End 에서 Visual Odometry 를 이용해서 이미지 정보를 기반으로 대략적인 카메라 움직임을 추정한다. 



특징


### ORB 특징점

ORB 특징점은 Keypoint + Descriptor로 구성된다. 

- KeyPoint : Oriented FAST 라고 불리며, 향상된 FAST 코너이다.
  - 이미지에서 '코너' 를 찾는다. 
  - FAST는 지역 픽셀 명암 스케일의 명벽한 변화를 감지하는 코너 포인트
    - 픽셀이 이웃의 픽셀과 크게 다를경우 코너가 될 가능성이 높다는 아이디어 
- Descriptor: BRIEF (Binary Robust Independent Elementary Feature)
- 



![orb_slam_sample](/images/2024-03-20-SLAM_theory1_Visual Odometry/orb_slam_sample.png){: .img-width-half .align-center}





![1d_local_7](/images/2024-03-20-SLAM_theory1_Visual Odometry/1d_local_7.png){: .img-width-half}

