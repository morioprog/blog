---
title: "【EDPC】O - Matching"
date: 2020-04-05T16:06:57+09:00
categories: ["精進"]
tags: ["DP", "bitDP"]
---

## [問題概要](https://atcoder.jp/contests/dp/tasks/dp_o)

$N$人の男と$N$人の女同士で$N$組のペアを作る通り数を$10^9 + 7$で割った余りを求めなさい.

但し, それぞれの男女には相性があり, $a_{i,j}$は男$i$と女$j$の相性を表すものとする.

### 制約

* $1 \leq N \leq 21$

## 考察

制約が**bitDP**だと言っているので従う.

とりあえず安直に考えると, 「$dp[i][msk] := i$人目の女と組み合わせる通り数 (男は$msk$だけ残っている)」となる.

だけどこれだと状態数が$O(N2^N)$, 遷移が$O(N)$なので全体で$O(N^22^N)$になってしまう (さすがに厳しい).

ここで, $msk$さえ分かれば$i$が求まることに着目すると状態数を$O(2^N)$に落とせる.

よって, 全体で$O(N2^N)$でこの問題は解けた.

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/dp/submissions/11571726)

```cpp
signed main() {
    SS(int, N);
    SVV(int, A, N, N);

    vector<int> bits(N, 0);
    REP(i, N) REP(j, N) bits[i] |= A[j][i] << j;

    vector<pair<int, int>> topo;
    REP(i, 1 << N) topo.emplace_back(__builtin_popcount(i), i);
    RSORT(topo);

    auto dp = make_v<modint>(1 << N);
    dp[(1 << N) - 1] = 1;

    REP(i, 1 << N) {
        int cnt, bit;
        tie(cnt, bit) = topo[i];
        int cand = bit & bits[N - cnt];
        REP(j, N) if ((1 << j) & cand) {
            int nxt = bit ^ (1 << j);
            dp[nxt] += dp[bit];
        }
    }
    print(dp[0]);
}
```
