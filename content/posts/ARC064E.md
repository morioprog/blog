---
title: "【ARC064】E - Cosmic Rays"
date: 2018-11-02T00:00:00+09:00
thumbnail: "img/header/atcoder.svg"
categories: ["精進"]
tags: ["グラフ"]
---

## [問題概要](https://atcoder.jp/contests/arc064/tasks/arc064_c)

平面座標全体に宇宙線が降り注いでいる. 座標上に中心$(x_i,y_i)$, 半径$r_i$の$N$個のバリアがあり, バリア内では宇宙線から逃れられる. このとき, $(x_s,y_s)$から$(x_t,y_t)$まで移動したときの, 宇宙線に晒される距離の最小値を求めよ.

### 制約

- $1 \leq N \leq 1000$
- $-10^{9} \leq x_s,y_s,x_t,y_t,x_i,y_i \leq 10^{9}$
- $1 \leq r_i \leq 10^{9}$

## 考察

まず, バリア間を移動する際の最短距離を考えてみると, それぞれの円の中心同士を結ぶ線上を移動するのが最短.

よって, そのバリア間で宇宙線に晒される距離は, `(中心間の距離)-(半径の和)`で求められる. (入力例 2 みたいに重なると負になるから, 0 との`max`をとる)

これで, 任意の 2 バリア間の距離が求められるから, グラフとして扱えそう.

距離は非負なので**ダイクストラ法**で求められる.

ただこれだと始点と終点の処理に困るから, それらも半径 0 のバリアとして扱っちゃえば問題ない.

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/arc064/submissions/3516953)

```cpp
int barrier[1010][3];
ld dis(int a, int b) {
    ld res = 0.0;
    res += sqrt(pow(barrier[a][0] - barrier[b][0], 2)
              + pow(barrier[a][1] - barrier[b][1], 2));
    res -= barrier[a][2] + barrier[b][2];
    if (res < 0) return 0;
    return res;
}

signed main() {
    int start = 0, goal = 1;
    REP(i, 2) {
        SS(int, x, y);
        barrier[i][0] = x;
        barrier[i][1] = y;
        barrier[i][2] = 0;
    }
    SS(int, N);
    REP(i, 2, N + 2) {
        SS(int, x, y, r);
        barrier[i][0] = x;
        barrier[i][1] = y;
        barrier[i][2] = r;
    }
    Graph g(N + 2);
    REP(i, N + 1) REP(j, i + 1, N + 2) add_edge(g, i, j, dis(i, j));
    vld v = Dijkstra(g, start);
    cout << setprecision(20) << v[goal] << ln;
}
```
