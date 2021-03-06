---
title: "【ARC071】F - Infinite Sequence"
date: 2020-04-09T13:12:00+09:00
thumbnail: "img/header/atcoder.svg"
categories: ["精進"]
tags: ["DP", "遅延セグメント木", "累積和", "行列"]
---

## [問題概要](https://atcoder.jp/contests/arc071/tasks/arc071_d)

各要素が$1$以上$N$以下の無限長の列のうち, 以下の条件を満たすものを数え上げなさい.

- 第$N$項以降はすべて同じ数
- 要素$a_i$に続く$a_i$個の要素はすべて同じ数

### 制約

- $1 \leq N \leq 10^6$

## 考察

どんな数列が条件を満たすかを考える.

- `111111111...`
- `322222222...`
- `233333333...`
- `211311123...`

ここから分かることについて, `1`かそうでないか(`#`とおく)で場合分けをすると

- `1`はそれ以降の数列に影響を与えない.
- `#`の次が
  1. `#`ならば, それ以降は一意
  1. `1`ならば, `#`個続いた後は不問

これを DP の遷移に落とせばよい.

### 遷移

「$dp[i] := $条件を満たす長さ$n$の数列の個数」と定義する.

**$i$番目の要素に`1`を入れるか`#`を入れるかで場合分けをする.**

- `1`を入れるとき, そのまま次に受け流せばよい.
  - `dp[i + 1] += dp[i]`
- `#`を入れるとき, そのあとが`1`か`#`かで再び場合分け
  - `#`ならば, それ以降は定まるので, `dp[N]`に足し込む.
    - `dp[N] += dp[i]`
  - `1`ならば, それ以降`#`が定まるので, `dp[i + # + 1]`に足し込む.
    - この`+1`は`3111`における`3`を指す.
    - `#`は$2$以上$N$以下なので, 区間加算を行う.
      - `dp[i + 2 + 1] ~ dp[i + N + 1] += dp[i]`

以上をまとめると, $i = 0, \ldots, N - 1$について, `dp[i]`から

- `dp[i + 1]`
- `dp[N]`
- `dp[i + 3 .. i + N + 1]`

に配ればよい.

答えは$\displaystyle\sum^{2N}_{i=N}{dp[i]}$.

### 蛇足

DP の遷移において, アクセスしうる要素の最大の index は`2 * N`.

いい感じに処理して$N$要素に抑えられるけどめんどくさかったので愚直に$2N$個持った...

### 解説を読んで

配らずに貰えば累積和が使えて, しかもそれを睨むと行列に落ちるのすごい.

そろそろ配る・貰うを意識して使い分けないと:thinking_face:

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/arc071/submissions/11651184)

```cpp
signed main() {

    SS(int, N);

    auto f = [](int x, int y) -> int { return (x + y) % MOD; };
    auto p = [](int x, int y) -> int { return ((ll)x * y) % MOD; };
    LazySegmentTree<int, int, decltype(f), decltype(f), decltype(f), decltype(p)> dp(
        2 * N + 1, f, f, f, 0, 0, vector<int>(N, 0), p
    );
    dp.update(0, 1, 1);

    REP(i, N) {
        int cur = dp[i];
        dp.update(i + 1, i + 2, cur);                                       // 1 x 1
        dp.update(i + 3, i + 3 + N - 1, cur);                               // (1以外) x 1
        if (i < N - 1) dp.update(N, N + 1, p(p(cur, N - 1), N - 1));        // (1以外) x (1以外)
    }

    print(dp.query(N, 2 * N + 1));

}
```
