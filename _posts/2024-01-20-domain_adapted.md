---
layout: single
title: "[Paper review] Domain adapted broiler density map estimation using negative-patch data augmentation"
categories: [AI, Paper Review]
tag: []
typora-root-url: ../
use_math: true


---

<br>

## [Domain adapted broiler density map estimation using negative-patch data augmentation](https://www.sciencedirect.com/science/article/pii/S1537511023001241)



**Author : Taehyeong Kim, Dae-Hyun Lee, Wan-Soo Kim, Byoung-Tak Zhang**





### **1. Introduction**



인구 증가 및 소득 증가의 영향으로 닭고기의 수요와 소비량은 꾸준히 증가함.

생산의 효율성을 위해 자유방목보다 폐쇄형으로 사육이 선호됨.

그에 따라 닭 사육은 높은 밀도로 사육됨.

높은 밀도로 사육됨에 따라, 행동이 제한돼 체온, 스트레스, 공포가 증가.

이에 따른 닭 품질 저하.

따라서 축산업계에서는 동물 복지 등을 통해 축산 품질 향상을 도모함.





해결하기 위한 사육 밀도를 관리하기 위해 사육 환경을 지속적으로 모니터링할 필요가 있음. 같은 수의 육계더라도, 사육환경과 성장 상태에 따라 밀도가 달라질 수 있음.





육계를 자동적으로 관찰하기위해 이전 연구들

1. 사육 방법별 6가지 파라미터를 이용한 육계 행동 평가
2. 사육 밀도에 따른 페킨 오리의 성장 및 활동 모니터링
3. 육계 복지 평가를 돕기 위한 백색 육계의 행동 분류
4. 중심체 분할 기반 동적 모델링을 이용한 체중 예측



그러나 이 연구들은 수만 마리 육계같은 혼잡한 무리의 모니터링에는 문제가 있음

자동으로 관찰하기 위해서는 고밀도 무리를 효과적으로 선별할 수 있는 기술이 필요함

딥러닝을 통해 물체를 자동으로 감지하는 솔류션이 많은 분야에서 적용됨

- CNN : 이미지 객체분류, 감지, 분할에 강력

  - 다양한 CNN 기반 아키텍쳐에 따라 사람, 사람 밀도를 평가하는데 사용되는 군중 수 계산 방법에 사용
  - 대규모 이벤트를 모니터링하여 군중 재난을 예방하는데 적용 --> 성공적인 밀도 맵 추정 & end to end learning : 뛰어난 성능

  > end to end learning : 입력에서 출력까지 파이프라인 네트워크 없이 신경망으로 한 번에 처리한다는 의미



- Density based counting method는 최근(2019)년에 양돈장의 돼지 수를 세는데 적용 <- 대규모 농업 생산 관리를 위해
-  Counting CNN model 을 기반으로 수정된 모델을 통해, 실제 환경에서 직접 수집하고 웹에서 다운로드한 데이터로 돼지 수를 예측
  - 373 image를 사용한 supervised learning, image당 평균 돼지 수는 약 15마리로 각 객체의 라벨링은 어렵지 않음.
  - --> MAE = 2.78 : 이전 crowd counting 연구와 비슷
- 그러나 일반적 육계 모니터링은 한 이미지에 최소 수백마리로, 일부 육계는 구분할 수 없음
  - 이러한 육계 이미지에 라벨링된 데이터 세트를 구축하는것은 사실상 불가능



**도메인은 다르지만, crowd counting과 broiler counting은 유사한 작업이다.**

도메인은 다르지만, 같은 작업을 할 때 전이 학습인 domain adaptation은 source 도메인(잘 훈련되고 라벨링된 많은 데이터 셋이 있는)을 target domain(라벨링이 잘 안된 데이터셋이 있는)으로  적용할 수 있기 때문에 유용하다. 

- DANN(Domain-Adversarial Training of Neural Networks)
  - reversal gradient layer , domain discriminator 추가
  - DANN-based domain adaptation은 plant organ 에 적용될 수 있다. --> 육계 counting에도 적용될 수 있지않을까?

broiler(를 포함한 농업분야에서)는 라벨링된 데이터가 부족한 것이 가장 중요한 문제

broiler 밀도 기반 지도(Density map based broiler)은 broiler 수 뿐만 아니라 flock distribution(무리 분포) 추정이 가능한데, 이는 성장 행동과 사육환경의 상관관계와 관련이 크기때문에 유용하다.

따라서 **DANN 기반 domain adaptation**을 사용하여, **Density map based broiler**를 추정하고 **broiler counting**을 목표로 연구한다.

domain간(source domain과 target domain)의 이질적인 object를 해결하기위해 negative-patch data augmentation strategy를 사용한다.

> negative-patch data augmentation strategy : source image의 임의 영역을 target image의 background로 대체 --> target domain의 distribution area 를 영역을 의도적으로 생성
>
> - 이 방법은 weakly-supervised support로 background information을 공유함으로써 추가 annotation cost없이 가능하다.



### **2. Materials and methods**
