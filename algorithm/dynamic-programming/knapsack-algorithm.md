---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
---

# KnapSack Algorithm

KnapSack Algorithm(배낭 문제)는 DP 알고리즘 중 하나다.

배낭 문제는 다음의 상황을 가진다.

> n개의 짐과 배낭이 있을 때, 각각의 짐은 가치와 무게가 있다.
>
> 배낭에 짐을 담을 수 있는 최대 용량이 존재할 때, 배낭에 담을 수 있는 가치의 최댓값을 찾아라.



이 문제가 왜 DP에 해당될까?

이 상황에서 가능한 모든조합은 $$2^n$$이다. 하지만 이 경우를 모두 살피기엔 너무 많은 시간이 소요된다.

그렇다면 DP를 고려했을 때, 부문제를 어떻게 정의해야 할지가 문제이다.

#### 최대 무게

가방의 무게를 기준으로 해결할 수 있는지 고려해보면 불가능하다.  왜냐하면 최대 무게가 4일때 최대 가치가, 최대 무게가 5일때 최대 가치로 이어지지 않는다. 짐을 빼고 다시 넣으면 아예 달라지기 때문이다.

#### 짐의 선택 여부

배낭에 짐을 넣을 때 두 가지 선택을 할 수 있다.

1. 짐을 배낭에 넣는다.
2. 짐을 배낭에 넣지 않는다.

만약 넣으려는 짐의 무게가 배낭의 무게를 초과한다면, 넣지 않는다. 하지만 짐을 넣을 수 있다고  해서 다 넣으면 만약 후에 배낭에 넣었을 때 더 가치가 높은 짐을 못 넣는 경우가 생긴다. 이 상황 또한 고려해야 한다. 그러면 결국 더 자세히 상황을 정리해야 한다.

* 새로운 짐을 넣지 않는다.
* 새로운 짐을 넣기 위해 현재 물건에 넣을 공간을 만들어서 물건을 넣는다.

이 상황을 수식으로 표현하면,&#x20;

`DP[i][w] = max(DP[i-1][w], DP[i][w-(짐 i의 무게)] + 짐 i의 가치`

**넣지 않는 경우**와 **짐을 넣는 경우**(현재 배낭에서 짐 i의 무게를 뺐을 때의 최대 가치에서 짐 i 의 무게를 더함) 중 최대값을 선택한다.

이 경우를 표로 비교해보자.

<table data-full-width="true"><thead><tr><th width="159">짐 \ 배낭 무게</th><th width="54">0</th><th width="52">1</th><th width="47">2</th><th width="47">3</th><th width="45">4</th><th width="51">5</th><th width="52">6</th><th>7</th></tr></thead><tbody><tr><td>1: 6 / 13</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>13</td><td>13</td></tr><tr><td>2: 3 / 6</td><td>0</td><td>0</td><td>0</td><td>6</td><td>6</td><td>6</td><td>13</td><td>13</td></tr><tr><td>3: 4 / 8</td><td>0</td><td>0</td><td>0</td><td>6</td><td>8</td><td>8</td><td>13</td><td>14</td></tr><tr><td>4: 5 / 12</td><td>0</td><td>0</td><td>0</td><td>6</td><td>8</td><td>12</td><td>13</td><td>14</td></tr></tbody></table>

위와 같이  값을 가지게 된다

3번 짐에서 배낭의 무게가 7인 경우를 살펴보자.

배낭의 무게가 6일 때 최대 가치는 13이다. 배낭의 무게가 7인 경우를 살필 때 3번 짐을 넣는 경우와 넣지 않는 경우를 살펴야 한다. 넣지 않는 경우는 이전값을 그대로 가져온다.

넣는 경우는 2번짐까지 고려하면서 배낭 무게가 3일 때의 최대 가치에서 3번 짐을 넣는 상황이 최대 가치가 된다. 따라서 비교는 `13 vs 6 + 8` 로 이루어진다.

즉, 위의 표를 2차원 리스트라고 생각한다면, 위처럼 수식화할 수 있다.

```python
dp[i][j] = max(dp[i-1][j], dp[i-1][j - item[i-1][weight]] + item[i-1][value])
```

dp는 위의 표를 2차원 배열로 나타낸 것이고, item 2차원 배열은 각 짐의 무게와 가치를 나타낸 것이다.