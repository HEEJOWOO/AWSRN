# AWSRN : https://arxiv.org/abs/1904.02358

#### <I studied while referring to various sites, but it is not enough. Thanks anytime for feedback.>
### <heejo5@naver.com>

Adaptive Weighted Super Resolution
--------------------------------------------------------------------------

* CNN기반 SR은 많은 계산량으로 실세계에 적용하기 어려움
* 경량화 네트워크인 AWSRN제안
* Local Fusion Block(LFB)으로 구성되어 있고 LFB는 Adaptive weighted residual units (AWRUs)와 Local Residual Fusion Units(LRFU)로 구성
* 복원 단계로 특징들을 최대한 활용하기위해 Adaptive weighted multi-scale(AWMS) 제안
* 딥러닝의 발전 → SR 성능의 많은 개선 → 네트워크의 깊이를 늘리며 성능을 올림 → 계산비용의 문제 발생 → 경량화의 중요성 제안 
![image](https://user-images.githubusercontent.com/61686244/108205349-4ef2bb00-7168-11eb-9d4c-073876da43ad.png)
* 경량화를하기 위해서는 얕은 네트워크이거나, 파라미터를 공유하는 재귀적인 방법 존재 → Operation 증가
* Residual Scaling 방법은 제한된 고정 가중치만을 사용하는 방법이라서 성능 향상에 제한
* 대부분의 SRNet의 복원 방법으로 사용되는 transposed, sub pixel 방법들은 전 단계인 non-linear mapping의 정보들을 불충분하게 사용함
* 다중 스케일 복원 방법은 더 많은 정보를 제공할 수 있지만 파라미터의 수가 늘어나게됨 
* AWSRN은 특징 추출단계와, non-linear mapping단계인 LFB으로 구성되어 있으며 LFB는 AWRUs과 LRFU으로 구성 되어 있음, 복원 단계로 제안하는 Adaptive weight multi scale 인 AWMS로 구성
Contribution
* non-linear mapping인 LFB은 AWRUs와 LRFU으로 구성되어 있고 AWRU는 정보와 gradient의 흐름을 효과적으로 사용하게 만듦, LRFU은 LFB의 다중 레벨 잔여 정보들은 융합하는데 효과적임
* AWMS는 nonlinear mapping으로부터 특징들을 최대한 사용하게끔 만들어 복원 성능을 향상 시킴, 중복된 scale branch는 가중치에 따라 제거할 수 있음 
* SOTA 네트워크들과 비교시 우수한 성능을 만들어 냈음
Efficient Residual Learning
* Residual Learning의 확장판 Weighted Residual Learning(WDSR)
* wSE는 복원 단계에서 우수한 성능을 만들긴 하였으나 추가적인 파라미터와 계산량이 증가
* Adaptive Weighted Residual Unit은 AWRU제안, AWRU는 wSE와 다르게 어떠한 추가적인 모듈 없이 독립적인 가중치를 사용 따라서 추가적인 계산량 증가 없음 
Network
![image](https://user-images.githubusercontent.com/61686244/108205741-d0e2e400-7168-11eb-84b6-763da344694c.png)
Upsampling Layer
* 복원단계에 사용하는 업샘플링은 SR에서 중요한 요소 중 하나 
* SRCNN, VDSR, DRRN과 같이 네트워크에 입력으로 보간된 LR을 사용하는 경우 계산량이 많이 증가
* FSRCNN, SRDenseNet에서 사용한 transposed 같은 경우 불필요한 계산량을 감소시켰음
* ESPCN에서 제안한 Sub pixel 같은 경우 transposed에서 발생하는 채커보드 문제를 해결 
* 하지만 transposed, sub pixel 같은 방법은 복원으로 single-scale module만을 사용하고, non linear mapping에서 특징 정보를 활용하는데 적합하지 못함 
* 제안하는 AWMS같은 경우 다중 scale conv를 사용하며 성능과 파라미터 사이의 어느 정도 절충을 가지고 있고 학습된 가중치에 따라 기여도가 낮은 일부 scale을 제거하여 성능 저하 없이 매개 변수를 줄일 수 있음 

Method
![image](https://user-images.githubusercontent.com/61686244/108206959-7185d380-716a-11eb-8e44-73ce0403afd6.png)

Local Fusion Block
* non linear mapping 단계로 LFB는 AWRU와 LRFU으로 구성
![image](https://user-images.githubusercontent.com/61686244/108207027-882c2a80-716a-11eb-811f-b6cdfd125d86.png)

* 그림 3의 (a)는 기본적인 residual unit을 의미하고 입력과 출력 차원을 축소하고 내부 차원을 확장, 추가적인 파라미터의 증가는 없음, 많은 low level정보를 사용 
* AWRU는 두 개의 독립적인 가중치를 가지고 있고 초기값을 받은 후 적응적으로 학습
![image](https://user-images.githubusercontent.com/61686244/108207198-c1fd3100-716a-11eb-975c-5d36c40c4db9.png)

* LFB의 AWRUs들의 특징 정보를 더 잘 살리기 위해 LRFU는 다중 level특징 정보를 융합함
Experiment
* Train : DIV2K / Test : Set5, Set14, Bsd100, Urban100, Manga109
![image](https://user-images.githubusercontent.com/61686244/108207285-e48f4a00-716a-11eb-9f80-bb63c94e909b.png)

![image](https://user-images.githubusercontent.com/61686244/108207293-e8bb6780-716a-11eb-970a-aa87aa8dfda3.png)

![image](https://user-images.githubusercontent.com/61686244/108207311-ece78500-716a-11eb-924e-71aea5e4402d.png)

![image](https://user-images.githubusercontent.com/61686244/108207327-f244cf80-716a-11eb-90e4-3d1b9035e7a8.png)

![image](https://user-images.githubusercontent.com/61686244/108207336-f53fc000-716a-11eb-83b6-30855de5a079.png)

* 3x3, 5x5 커널이 퀄리티에 영향을 많이 끼침,  3x3은 저주파수를 다른 큰 커널들은 고 주파수에 영향을 많이 끼침
* scale branch 마다 다른 기여도를 가지고 있고 즉 다양한 기능 정보를 가질 수 있음을 의미
![image](https://user-images.githubusercontent.com/61686244/108207368-012b8200-716b-11eb-9434-ec95a40549c6.png)

![image](https://user-images.githubusercontent.com/61686244/108207378-04bf0900-716b-11eb-811c-8e1ea133eca5.png)

![image](https://user-images.githubusercontent.com/61686244/108207389-07b9f980-716b-11eb-8df8-af5187f75bc6.png)
* LFB를 제안하여 효과적인 잔여학습을 할 수 있도록 만들었으며, LFB의 AWRU와 LRFU는 정보와 gradient를 효과적으로 흐를 수 있게 만들었음
* AWMS는 복잡한 정보를 최대한 활용하고 서로 다른 scale branch사이의 정보 중복성을 분석하여 파라미터의 수를 줄임 










