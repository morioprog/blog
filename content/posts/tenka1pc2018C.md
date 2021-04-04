---
title: "【Tenka1 Programmer Contest】C - Align"
date: 2018-11-02T00:00:00+09:00
categories: ["精進"]
tags: ["貪欲"]
---

## [問題概要](https://atcoder.jp/contests/tenka1-2018/tasks/tenka1_2018_c)

$A_1,A_2,\ldots,A_N$の$N$個の整数を好きな順番に並べたとき, 隣り合う要素の差の合計の最大値を求めよ.

### 制約

- $2\leq N\leq 10^{5}$
- $1\leq A_i\leq 10^{9}$

## 考察

とりあえず$N$個の整数をソートする.

最小の数字と最大の数字は隣同士であってほしいお気持ちになったので, 一旦そこを並べる.

次は最小の方に 2 番目に大きい数字を, 最大の方に 2 番目に小さい数字を並べる. こんな感じで貪欲に差を大きくしていけば良さそう.

$N$が奇数だったら最後 1 個余るけど, そこも貪欲に置けばいい.

ex.) $1,2,3,6,8$

1. $1, 8 \rightarrow res = 8 - 1 = 7$
1. $6, 1, 8, 2 \rightarrow res = 7 + (6 - 1) + (8 - 2) = 18$
1. $3, 6, 1, 8, 2 \rightarrow res = 18 + (6 - 3) = 21$

よって, 21 と求まった.

## [提出コード(Python3:snake:)](https://atcoder.jp/contests/tenka1-2018/submissions/3479452)

```python
n = int(input())
a = sorted([int(input()) for _ in range(n)])
res = a[-1] - a[0]
mn, mx = a[0], a[-1]
for i in range(1, n // 2):
    res += mx - a[i]
    res += a[-1 - i] - mn
    mn, mx = a[i], a[-1 - i]
if n % 2 == 1:
    res += max(abs(mx - a[n // 2]), abs(mn - a[n // 2]))
print(res)
```
