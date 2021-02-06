**Automatic Music Playlist Continuation via Neighbor-based
Collaborative Filtering and Discriminative
Reweighting/Re-ranking**

[Automatic Music Playlist Continuation via Neighbor-based Collaborative Filtering and Discriminative Reweighting/Reranking](https://dl.acm.org/doi/pdf/10.1145/3267471.3267481?casa_token=UvP1zJ-j4u4AAAAA:NOEupgUVc4tZVZ_vk_LFDwss6BkueZHZ2T9v5KH81yLDflE4P5M1iAl7XVMfZjqAsiE_CrcVArZF)

# **3. Our Solution**

**Neighbor-based CF의 Main idea**

- `song s`를 포함한 `playlist p`가  `playlist u`와 유사하다면, `playlist u`에게 `song s`를 추천해야한다.
- `playlist p`에 포함된 `song s`와 유사한 `song t`를 `p`에게 추천해야 한다.

🎶 **i-th song의 embedding s{i,u} 구하기**

주어진 playlist u에 대해서, 모든 노래 s의 feature vector를 다음 규칙에 따라 정의한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6703ed64-11ac-405b-a43e-43a9a88440dc/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6703ed64-11ac-405b-a43e-43a9a88440dc/Untitled.png)

`alpha` ; hyperparameter in [0,1] (control the influence of long playlist)

`song i`가 속한 ply들과 `ply(u)`와의 교집합 점수를 구해서 `song{i,u}`를 만든다.

- 같은 노래를 가지고 있지만 다른 곡은 유사하지 않은 ply의 점수는 낮아지고
- 같은 노래를 가지면서 다른 곡까지 유사한 ply와의 점수는 높아진다.

🎶 **i-th song과 j-th song의 simliarity**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c6d34809-7e6c-43e0-854e-c3fef07e91d1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c6d34809-7e6c-43e0-854e-c3fef07e91d1/Untitled.png)

`beta` ; hyperparameter in [0,1]

i는 후보군, j는 target playlist ply(u)에 속한 노래.

🎶 **ply(u)에 대한 i-th song의 최종 rating**

ply(u)에 속한 노래와 후보군 노래 i의 sim을 구한 후, ply(u)의 크기로 나누어 최종 점수로 사용한다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eafea515-44ad-42b1-9507-cd413bb6116f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eafea515-44ad-42b1-9507-cd413bb6116f/Untitled.png)
