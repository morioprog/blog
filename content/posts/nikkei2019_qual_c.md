---
title: "【全国統一プログラミング王決定戦予選】C - Different Strokes"
date: 2020-04-10T08:38:37+09:00
thumbnail: "img/header/nikkei.svg"
categories: ["精進"]
tags: ["比較関数"]
---

## [問題概要](https://atcoder.jp/contests/nikkei2019-qual/tasks/nikkei2019_qual_c)

$N$個の料理がある.

料理$i$を食べると, 高橋くんは幸福度$A_i$を, 青木さんは幸福度$B_i$を得る.

それぞれ`(最終的に自分が得る幸福度の総和) - (最終的に相手が得る幸福度の総和)`を最大化するように動く.

このとき, `(最終的に高橋くんが得る幸福度の総和) - (最終的に青木さんが得る幸福度の総和)`を求めなさい.

### 制約

- $1 \leq N \leq 10^5$
- $1 \leq A_i \leq 10^9$
- $1 \leq B_i \leq 10^9$

## 考察

高橋くんがどのような優先順位で皿を取るべきかを考える.

$(A_1, B_1)$の皿 1 と, $(A_2, B_2)$の皿 2 について

1. 皿 1 → 皿 2 の順に取るとき : $A_1 - B_2$分の優位を得る.
1. 皿 2 → 皿 1 の順に取るとき : $A_2 - B_1$分の優位を得る.

前者の方が取るべきであるとき, $A_1 - B_2 > A_2 - B_1$, すなわち$A_1 + B_1 > A_2 + B_2$であることが分かった.

同様の議論を青木さん側でも行え, 同様の結果が得られる.

よって, $A_i + B_i$の値によってソートすればよい.

## [提出コード(Python:snake:)](https://atcoder.jp/contests/nikkei2019-qual/submissions/4111739)

```python
res = 0
n = int(input())
l = []
for i in range(n):
    a, b = map(int,input().split())
    l.append([a + b, a, b])
l = sorted(l)[::-1]
for i in range(n):
    if i % 2 == 0:
        res += l[i][1]
    else:
        res -= l[i][2]
print(res)

```
