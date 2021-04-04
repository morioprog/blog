---
title: "【ABC103】D - Islands War"
date: 2018-08-30T00:00:00+09:00
thumbnail: "img/header/atcoder.svg"
categories: ["精進"]
tags: ["区間スケジューリング"]
---

## 問題概要

東西に一列に並ぶ$N$個の島と, それをつなぐ$N - 1$個の橋がある.

「$a_i$と$b_i$の間を行き来できないようにしろ」という要望が$M$個寄せられた.

取り除かなければならない橋の本数の最小値を求めよ.

### 制約

- $2 \leq N \leq 10^5$
- $1 \leq M \leq 10^5$
- $1 \leq a_i < b_i \leq N$

## 考察

$b_i$をソートして, それぞれの$b_i$の左隣の橋を崩していくのが最適っぽい. (**区間スケジューリング**っていうらしい)

とりあえず要望を`vector<pair<int,int>>`にぶち込んで, `second`でソートする.

その`second`を順番に辿って行って, 隣の橋($b_i - 1, b_i$間)を崩したときに要望を達していたらそれを$(-1, -1)$にして判定の対象外にして..., を繰り返す.

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/abc103/submissions/3101715)

```cpp
signed main() {
    SS(int, N, M);
    vpii request;
    REP(i, M) {
        SS(int, a, b);
        request.PB(MP(--a, --b));
    }
    int res = 0;
    sort(all(request), SORT_SECOND);
    REP(i, M) {
        if (request[i].first < 0) continue;
        int x = request[i].second;
        REP(j, i, M) {
            if (between(request[j].first, 0, x)) {
                request[j].first  = -1;
                request[j].second = -1;
            }
        }
        ++res;
    }
    cout << res << endl;
}
```
