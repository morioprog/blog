---
title: "【Educational Codeforces Round 51】D - Bicolorings"
date: 2018-09-21T00:00:00+09:00
categories: ["精進"]
tags: ["DP"]
---

## [問題概要](http://codeforces.com/contest/1051/problem/D)

$2$列$n$行のマス目を白黒に塗り分ける. 塗り分けた後, 区画が$k$個できるような塗り分け方の数を求めよ.

結果はかなり大きい数になるため、$998244353$で割ったあまりを出力せよ.

### 制約

- $1 \leq n \leq 1000$
- $1 \leq k \leq 2n$

## 考察

ぱっと見 DP っぽいので DP をする.

(黒,黒),(黒,白),(白,黒),(白,白)の 4 通りを毎行考えて, 区画が増える場合とそうでない場合に分けてそれぞれ漸化式を考える.

`dp[何行目まで見た][そこまでの区画数][左の塗り分け][右の塗り分け]`とすると, 求めるのは$\displaystyle\sum\_{a=0}^{3} \sum\_{b=0}^{3} dp[n-1][k][a][b]$.

## [提出コード(C++:high_brightness:)](http://codeforces.com/contest/1051/submission/43163168)

```cpp
/* (0, 1, 2, 3) = ((黒, 黒), (黒, 白), (白, 黒), (白, 白)) */

const int MOD = 998244353;
#define ADD(a,b) a=(a+ll(b))%MOD

ll dp[1010][2020][4][4];

signed main() {

    SS(int, N, K);

    // 初期値
    dp[0][1][0][0] = 1;
    dp[0][2][0][1] = 1;
    dp[0][2][0][2] = 1;
    dp[0][1][0][3] = 1;

    REP(i, N - 1) REP(j, 1, 2 * N + 1) REP(a, 4) REP(b, 4) REP(c, 4) {
        if (b == 0 or b == 3) {
            if (b == c) ADD(dp[i + 1][j][b][c], dp[i][j][a][b]);
            else ADD(dp[i + 1][j + 1][b][c], dp[i][j][a][b]);
        } else {
            // (b, c) = (1, 2), (2, 1)
            if (b * c == 2) ADD(dp[i + 1][j + 2][b][c], dp[i][j][a][b]);
            else ADD(dp[i + 1][j][b][c], dp[i][j][a][b]);
        }
    }

    ll res = 0;
    REP(a, 4) REP(b, 4) ADD(res, dp[N - 1][K][a][b]);
    cout << res << endl;

}
```
