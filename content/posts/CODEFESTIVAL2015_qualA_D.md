---
title: "【CODE FESTIVAL 2015】D - 壊れた電車【予選A】"
date: 2018-08-23T00:00:00+09:00
categories: ["精進"]
tags: ["二分探索"]
---

## 問題概要

それぞれ$X_i$両目の車両にいる$M$人の整備士が, $N$両編成の電車をすべて点検し終えるのに最短で何分かかるか求めよ.

但し, 点検には時間はかからないが, 隣の車両に移動するのには1分かかるものとする.

### 制約

* $1 \leq N \leq 10^9$
* $1 \leq M \leq 10^5$
* $M \leq N$
* $1 \leq X_i \leq N$

## 考察

### 方針

求める値を$ans$として, $ans$を$0,1,2,\ldots$と地道に増やしていくのだと, それだけで$O(N)$だから間に合わなさそう.

$ans$を二分探索すれば, そこは$O(\log N)$で収められる.

### 点検できるかの判定

左(車両番号の小さい方)の整備士から順に点検していくとして, 最終的な位置がなるべく右にあった方がうれしい. $l$両目まで点検済みで, $c$両目にいる整備士が$t$分点検するとする.

* $c \leq l$ のとき, 整備士は$c + t$両目まで点検できるため, $l = \mathrm{max}(l, c + t)$.
* そうでないなら, **左を経由して右に進む場合**と**右を経由して左に進む場合**をそれぞれ考えて, より右に進んだ方を選べばよい. 但し, 最終位置の左側に点検し残した部分がある場合, すなわち$c-(l+1)>t$のとき, すべて点検し終えることは絶対にできないので省く.

左を経由して右に進む場合, `(c) → (l + 1) → (r)`と移動するため, $(c-(l+1))+(r-(l+1))=t$, 整理して**$r=t-c+2(l+1)$**.

右を経由して左に進む場合, `(c) → (r) → (l + 1)`と移動するため, $(r-c)+(r-(l+1))=t$, 整理して**$\displaystyle r=\frac{t+c+l+1}{2}$**.

ゆえに, $\displaystyle l = \mathrm{max}(t - c + 2(l + 1), \frac{t + c + l + 1}{2})$

この操作をすべての$c$に対して行い. 最終的に**$l \geq N - 1$**となれば点検が完了していると分かる.

点検部分は多分$O(M)$なので, 合わせて$O(M\log N)$だから間に合いそう.

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/code-festival-2015-quala/submissions/3062027)

```cpp
#define reps(i,a,b) for(int (i)=(a); (i)<(b); ++(i))
#define rep(i,n) reps(i,0,(n))
#define chmax(x,y) x=max(x,y)

ll N, M, X[100100];

bool tenken(ll minute) {
    ll left = -1;
    rep(i, M) {
        ll c = X[i] - 1;
        if (c <= left) {
            chmax(left, c + minute);
        } else {
            if (c - left - 1 > minute) return false;
            ll r1 = minute - c + 2 * (left + 1);
            ll r2 = (minute + c + left + 1) / 2;
            chmax(left, max(r1, r2));
        }
    }
    return left >= N - 1;
}

signed main() {
    cin >> N >> M;
    rep(i, M) cin >> X[i];
    ll low = 0, high = 2 * N;
    while (low <= high) {
        ll mid = (low + high) / 2;
        if (tenken(mid)) {
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    cout << low << endl;
}
```
