# Training Deep AutoEncoders for Collaborative Filtering

> 리뷰 https://soobarkbar.tistory.com/124



##### REVIEW

* [1]()  흐름 이해용

* [2](http://samediff.kr/wiki/index.php/Training_Deep_AutoEncoders_for_Collaborative_Filtering#cite_ref-2)  구조 설명이 잘 되어있음



#### SUMMARY

> `SELU` `Re-feeding`  `Masked MSE`

* **Activation f를 ELU계열(논문에서는 SELU)**을 쓰는 것과
  **Negative part를 없애지 않는 것**이 학습에 결정적인 역할을 한다.



* activation f의 범위가 output보다 작으면, 마지막 layer는 activation을 쓰지 말야아한다 ! 그냥 바로 linear하게 연결





##### LOSS ; Masked MSE

* 실제 rating이 있는 경우에 대해서만 loss를 측정하겠다 !
* github에선 def시에는 input/target이면서 ,,, 실제로 run.py에서 사용은 뒤집어서 넣었네 헷갈리게 !!

