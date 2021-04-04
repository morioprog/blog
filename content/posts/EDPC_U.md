---
title: "【EDPC】U - Grouping"
date: 2020-04-05T17:29:32+09:00
categories: ["精進"]
tags: ["DP", "bitDP", "部分集合列挙"]
---

## [問題概要](https://atcoder.jp/contests/dp/tasks/dp_u)

$N$羽のうさぎをいくつかのグループに分けたい.

うさぎ$i$とうさぎ$j$の相性は$a_{i, j}$である.

各グループのスコアを, そのグループに属する任意の 2 羽のうさぎの相性の和とする.

スコアの最大値を求めなさい.

### 制約

- $1 \leq N \leq 16$
- $a_{i, i} = 0$
- $|a_{i, j}| \leq 10^9$
- $a_{i, j} = a_{j, i}$

## 考察

制約が**bitDP**だと言っているので従う.

DP の定義を考える. 「$dp[msk] := msk$を分けるときの答え」とすると, 求めるべきは$dp[(1 \<\< N) - 1]$となる.

$msk$の$popcount$が$1$なら自明に$dp[msk] = 0$.

そうじゃないなら, $\displaystyle dp[msk] = \max_{y \subseteq msk} ( dp[y] + dp[y\ \mathrm{XOR}\ msk] )$のように部分集合を列挙すればよい.

愚直に部分集合を列挙すると$O(4^N)$だけど, いい感じにすると$O(3^N)$に落ちる([参考](#ref)).

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/dp/submissions/11574807)

```cpp
int N;
vector<vector<int>> A;

vector<ll> memo;
ll calc(int msk) {
    if (memo[msk] != -INFF) return memo[msk];
    ll ret = 0;
    REP(i, N) if (msk & (1 << i)) REP(j, i, N) if (msk & (1 << j)) ret += A[i][j];
    return memo[msk] = ret;
}

vector<bool> visited;
vector<ll> dp;
ll rec(int msk) {
    if (visited[msk]) return dp[msk];
    visited[msk] = true;
    if (__builtin_popcount(msk) == 1) return dp[msk] = 0;
    ll ret = calc(msk);
    for (int y = 0; ; y = (y - msk) & msk) {    // 参考
        if (y == msk) break;
        int y_rev = msk ^ y;
        chmax(ret, rec(y) + rec(y_rev));
    }
    return dp[msk] = ret;
}

signed main() {
    cin >> N;
    A = make_v<int>(N, N);
    REP(i, N) REP(j, N) cin >> A[i][j];

    memo.assign(1 << N, -INFF);
    visited.assign(1 << N, false);
    dp.assign(1 << N, -INFF);
    cout << rec((1 << N) - 1) << endl;
}
```

<a id="ref"></a>

## 参考

- [bit 演算による集合の列挙 - うさぎ小屋](https://kimiyuki.net/blog/2017/07/16/enumerate-sets-with-bit-manipulation/)
