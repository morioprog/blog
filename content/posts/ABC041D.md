---
title: "【ABC041】D - 徒競走"
date: 2019-02-06T00:00:00+09:00
thumbnail: "img/header/atcoder.svg"
categories: ["精進"]
tags: ["DP", "bitDP"]
---

## [問題概要](https://atcoder.jp/contests/abc041/tasks/abc041_d)

$N$匹のうさぎがいる. $M$人の観客から, うさぎ$x_i$がうさぎ$y_i$よりも先にゴールしたという情報を得た.

これら全ての情報に合致する着順が何通りあるかを求めよ.

### 制約

- $2\leq N\leq 16$
- $1\leq M\leq \frac{N(N - 1)}{2}$

## 考察

**bitDP**をする. `dp[何匹見たか][状態]`としたとき, 各うさぎがすでにゴールしたかを$N$bit の状態として持つ (bit が立っていればすでにゴールしているとする).

### 遷移

`dp[n][stat]`からの遷移を考える. 次に遷移する候補は, `stat`の bit が立っていないものから選ぶ.

候補のうさぎを$p$とすると, 情報と照らし合わせたとき

- $x_i=p$なら, $y_i$はまだゴールしていないはず.
- $y_i=p$なら, $x_i$はすでにゴールしているはず.

この 2 つの条件を満たしていないと, 情報に合致していないと分かり, 遷移を行わない.

満たしているときは, `dp[n + 1][stat + (1 << p)] += dp[n][stat]`として配っていけば良い.

よって遷移に$O(M)$かかって, 全体の計算量は, $O(MN^{2}2^{N})$.

ただ, 現状この計算量だと最悪で$10^{9}$を超えてしまうので, 遷移する際に無駄な情報を見ない(うさぎ$p$に関する情報だけを集約する)ようにしてごまかした.

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/abc041/submissions/4182057)

```cpp
ll dp[22][77777];
vl after[22], before[22];

signed main() {
    SS(ll, N, M);
    REP(i, M) {
        SS(ll, x, y);
        --x, --y;
        after[x].PB(y);
        before[y].PB(x);
    }

    dp[0][0] = 1;
    REP(n, N) {
        REP(stat, (1LL << N)) {
            // まだゴールしてないなら, dp[n][stat]からdp[n+1][stat+(1<<i)]に遷移できるか確認
            REP(i, N) if ((stat & (1LL << i)) == 0) {
                bool canTransit = true;
                EACH(aft,  after[i]) if ((stat & (1LL << aft)) != 0) canTransit = false;
                EACH(bef, before[i]) if ((stat & (1LL << bef)) == 0) canTransit = false;
                if (!canTransit) continue;
                dp[n + 1][stat + (1LL << i)] += dp[n][stat];
            }
        }
    }
    print(dp[N][(1LL << N) - 1]);
}
```
