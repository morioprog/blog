---
title: "【codeFlyer】C - 徒歩圏内"
date: 2019-04-18T00:00:00+09:00
categories: ["精進"]
tags: ["二分探索", "累積和"]
---

## [問題概要](https://atcoder.jp/contests/bitflyer2018-qual/tasks/bitflyer2018_qual_c)

$N$個の都市がそれぞれ座標$X_i$にある. 2 つの都市間の距離が$D$以下であれば徒歩で, そうでなければ電車で移動する.
このとき, 3 つの都市の組$(i, j, k)$であり, 以下の条件を満たすものの個数を求めよ.

- $i < j < k$
- 都市$i$と都市$j$, 都市$j$と都市$k$を徒歩で移動する.
- 都市$i$と都市$k$を電車で移動する.

### 制約

- $3 \leq N \leq 10^5$
- $0 \leq X_i \leq 10^9$
- $0 \leq D \leq 10^9$

## 考察

**二分探索**がほのかに香ってきたのでその方針で考える.

都市$i$から都市$j$, 都市$j$から都市$k$まで徒歩で移動する場合の数から, 都市$i$から都市$k$まで徒歩で移動する場合の数を引けば良さそう. 前者を$A$, 後者を$B$と置いて求め方を考えていく.

### $A$について

都市$i$を固定する. 都市$i$から徒歩で行ける都市$j$全てについて, さらにその都市$j$から徒歩で行ける都市$k$の数を数えれば良い.

それぞれの都市から徒歩で行ける都市の数をあらかじめ数えておく.

これはそれぞれの都市について, どの都市まで徒歩で行けるかを二分探索することで求められる($O(N \log N)$). (変数名は$walk$)

次に, それぞれの都市から徒歩で行ける都市について, $walk$の和を順に足していきたい. 徒歩で行ける都市はもちろん並んでいるので, $walk$の累積和を取ることでオーダーを落とせる.

よって, どの都市まで徒歩で行けるかを二分探索で調べ, その$walk$の区間和を累積和で求めることにより$A$が求められた($O(N \log N)$).

### $B$について

都市$i$を固定する. そこから徒歩で行ける都市の数を二分探索で求める. それらの都市から適当に 2 つ選べば$B$の条件を必ず満たすので, $B$が求められた($O(N \log N)$).

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/bitflyer2018-qual/submissions/5019124)

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
#define REP(i,n) for(int (i)=0; (i)<(n); ++(i))
#define LEQ(v,x) (upper_bound((v).begin(), (v).end(), x) - (v).begin())

int N, D;
ll res;
vector<ll> X, walk, walk_acc;

ll comb(ll num) {
    return max<ll>(0, num * (num - 1) / 2);
}

int main() {

    cin >> N >> D;
    X.resize(N);
    REP(i, N) cin >> X[i];

    REP(i, N) walk.push_back(LEQ(X, X[i] + D) - i - 1);

    walk_acc.push_back(0);
    REP(i, N) walk_acc.push_back(walk_acc[i] + walk[i]);

    REP(i, N) {
        res += walk_acc[min<ll>(N, LEQ(X, X[i] + D))] - walk_acc[i + 1];
        res -= comb(LEQ(X, X[i] + D) - i - 1);
    }

    cout << res << endl;

}
```
