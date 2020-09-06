`0906`



# :scroll: Long-tail Session-based Recommendation [Link](https://arxiv.org/pdf/2007.12329.pdf)

Session based는 이전부터 있던 개념. 이 논문은 Long-tail이 핵심 !

`Soft adjust` `Preference Mechanism`



## What is Session-based Recommendation?

> `Session` : a temporary and interactive information interchange between two or more communicating devices, or between a computer and user

* User가 입장해서 나가기까지 탐색한 item 정보를 이용해서 다음에 click/구매할 item을 맞춰 추천하자.
* User 정보를 이용하는 것이 아니라, **현재 접속해서 만들어진 내역**만을 이용한다.



## Long-tail에 집중한 추천을 하자

#### Head? Tail?

* 파레토 법칙 : 상위 20%가 전체 80%를 차지한다.
* 인기있는 상위 20% item을 head, 나머지를 tail이라고 하자.



#### 왜 잘 팔리지 않는 Long-tail item을 노출시키고 싶을까?

1. `Diversity` : 인기 item만을(Head만을) 자주 추천해준다면, 추천이 **지루해진다.**
   * Long tail item을 통해 새로운 경험을 제시한다.
   * 단 쌩뚱맞은 것이 아니라 관련이 있지만 생소한 item을 추천하고싶다.
2. `Business Side` 이미 인기있는 item이라면, 우리가 굳이 추천해주지 않아도 user들이 알아서 찾아볼 것이다.
   * User들에게 새로운 item을 소비하게 만듦으로써 비즈니스상으로 이익을 얻고싶다.



## Obstacles in Session-based Recommendation

1. session에서는 side information(user의 성별, 나이 등)이 부족하다.
2. 이전의 연구들은 Sequential Order, 즉 user가 탐색한 item들의 순서를 고려하지 않고있다.
3. 같은 User가 여러번 들어오더라도 Session은 독립적으로 취급되므로 다른 data들보다 Sparsity가 높다.
4. 이전의 연구들은 Accuracy를 손해보면서 Long tail을 추천했다.

##### => objective : Session-based에서 long tail을 추천하면서도 Acc를 손해보지 말자 !



## Architecture ; TailNet

![image-20200906211833963](C:%5CUsers%5Chaeyu%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200906211833963.png)



#### Feature Representation

> Feature Representation 모델은 다른 것으로 바꾸어도 상관 없다. 이 논문에서 강조하고 싶은 부분은 Soft adjustment이다.

1. GRU Encoder로 각 item에 대한 hidden score를 뽑아낸다.

2. Attention을 거친다. (self-attention아님)
3. 전체에 대한 global pooling

3. 가장 마지막 item의 hidden과 concat

4. 전체 dimension과 같도록 linear transformation



#### Soft Adjustment

5. tail item에는 one-vector를, head item에서는 zero-vector를 더해서(Item type을 representation하기 위해)
6. 역시 Attention을 거치고
7. Sigmoid를 통해 R~head~ 값을 얻는다.
   * R~tail~ = 1 - R~head~
8. 4에서 나온 vector의 Head 부분에는 R~head~을, Tail 부분에는 R~tail~을 곱해서 softmax를 거친다.
   * R~head~와 R~tail~ 을 통해 `soft adjustment`를 한다고 말한다.



## Experiments

* 아래의 feature representation 모델은 다른 것으로 바꾸어도 상관 없다. 아래 모델만 바꾸어 SOTA에 적용해본 결과 Acc는 유지하면서도 Long tail의 추천 비율이 늘어났다.



## Ablation Study

##### 너네 모델이 잘 된 이유는 파레토 법칙이라는 Prior Knowledge가 있었기 때문아니야?

* 아니. 우리는 soft adjustment덕분이야.
* soft adjustment를 하지 않고 head중에서 k개, tail중에서 n-k를 추천해줬는데 성능이 엄청 떨어지더라 ! 즉 파레토 법칙 때문은 아니야.



## :thinking: Discussion

#### Attention(self아님)시에 마지막에 소비된 item을 따로 고려한 이유는 무엇일까?

##### 스터디원분의 답변

session based RS에서 사용하는 데이터셋들은 last item에 대한 dependency가 굉장히 강하기 때문에 last item만 보고도 꽤 높은 정확도가 나온다. SOTA method들도 비슷한 attention 방식을 쓰고있다.



#### R~head~ 와 R~tail~ 의 계산이 statistic에 기반한 것보다 Attention을 이용한 것의 성능이 더 좋다는 실험은 왜 없는가?

논문에서 포인트를 둔 부분은 아님. adjustment가 효과가 있는가 없는가에 초점을 두었음.
