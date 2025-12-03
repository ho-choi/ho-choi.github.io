---
layout: single
title: "[LLM] Fine Tuning" 
categories: LLM
typora-root-url: ../
toc: true
---

### 목표
---

Fine Tuning에 대해서 알아보고, 실습해본다



### 정의

---

Fine-tuning은 **이미 학습된 대규모 모델(Pretrained Model)** 에  **내가 가진 도메인 데이터**를 추가 학습시켜서  **특정 작업에 더 잘 맞도록 만드는 과정**이다. 





PEFT 란?

Parameter Efficient Fine Tuning

적은 매개변수 학습으로 , 빠른 시간에 새로운 문제를 해결하는 Fine-Tuning 방법

다양한 언어와 도멘인의 데이터 모델을 적용할 때 유용

각 도메인 또는 언어별로 작은 체크 포인트만 로컬에 저장하면 효율적으로 작동

다양한 태스크에 모델을 신속히 적용할 때 유용



