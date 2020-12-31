# Collaborative Filtering for Implicit Feedback Datasets

> [paper link](http://yifanhu.net/PUB/cf.pdf)



### Alternating Least Squares

##### user와 item을 번갈아 고정시키고 학습하자.

* p,q를 학습시마다 매번 모두 미분하여 backprop하는 것이 아니라, 둘 중 하나를 고정시키고 교대로 update시킨다.



##### { *NOTATION* }

* ***x~u~*** : user- pseudo item 에서 추출한 user vector (SVD)

* ***y~i~*** : item - pseudo user 에서 추출한 item vector (SVD)

* ***b~u~*** : user와 관련된 bias , ***b~i~*** : item과 관련된 bias

* ***r~ui~ =  x~u~^T^ y~i~ + b~u~ + b~i~*** : user `u`가 item `i`를 선호하는가?에 대한 점수
  ( 여기서는 implict feedback 점수 ! )



### :point_up: 점수에 기반한 신뢰 정도를 이용하자 !

> http://yifanhu.net/PUB/cf.pdf

우리가 측정한 암묵적 피드백 점수 r~ui~ 를 완전히 믿을 수 없으니까 ... 어느정도만 반영해서 모델을 만들어볼까?



##### implict feedback에 대한 indicator를 정의하자.

* ***p~ui~ = sign(r~ui~)*** : user `u`가 item `i`를 선호하는가?를 1/0으로 나타낸다. 
  * sign 함수를 이용하여 이전에 구매하지 않은 제품이라면 0, 한번이라도 구매한 적이 있다면 1을 부여한다.
  * 점수가 모두 0 이상이라는 점을 이용해서 one-hot encoding을 했다고 생각하면 된다.



##### 흔적이 있으니 분명 선호할거야. 그렇지만 alpha만큼만 반영하자!

* ***c~ui~ =  1 + alpha  r~ui~*** : user `u`가 item `i`를 선호하는 점수를 어느 정도만 반영한 새로운 점수 !

  * 여러번 재구매 했다면 해당 아이템을 선호할 확률이 올라간다. 즉 재구매 횟수와 선호 level사이에는 양의 상관 관계가 있을 것이다.

가장 간단한 증가함수인 linear를 적용해보자.
    

  * 1을 통해 기본 점수를 주고, alpha  r~ui~를 통해 implict feedback을 반영해준다.



##### hyper parameter인 alpha가 주는 효과

1. alpha를 조절하여, 우리가 제시한 점수인 r~ui~ 를 얼마나 반영할 것인지 조절할 수 있다.

2. alpha를 줄여서 0이 아닌 r~ui~ 와 0 사이의 차이를 좁혀준다.

   (이를 positive와 negative observation의 <u>중요도를 조절</u>한다고 말한다.)



원래 우리가 optimize해야할 함수는 다음 형태이다.

<img src="../../fig/image-20200422231110764.png" alt="image-20200422231110764" style="zoom:80%;" />

그러나 c~ui~ 를 반영하여 약간 변형된 RMSE를 optimize한다.

<img src="../../fig/image-20200422231324113.png" alt="image-20200422231324113" style="zoom:80%;" />

* c~ui~가 작다면, p~ui~ 와 추정값 x~u~ ^T^ y~i~ 사이에 error가 크더라도, c~ui~ 가 큰 경우보다 영향력이 적다. 



##### 학습시에 GD가 아닌 ALS를 이용하면 Linear Time에 학습이 종료된다.



