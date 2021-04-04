---
title: "【ABC115】D - Christmas"
date: 2018-12-08T00:00:00+09:00
categories: ["精進"]
tags: ["再帰"]
---

## [問題概要](https://atcoder.jp/contests/abc115/tasks/abc115_d)

レベル$L$のバーガーを以下のように定義する.

- レベル$0$のバーガーは, `P`.
- レベル$L$のバーガー($L \geq 1$)は, `B` + (レベル$L - 1$のバーガー) + `P` + (レベル$L - 1$のバーガー) + `B`.

このとき, レベル$N$のバーガーの下から$X$層までに`P`は何個あるか求めよ.

### 制約

- $1 \leq N \leq 50$
- $1 \leq X \leq ($レベル$N$のバーガーの層の総数$)$

## 考察

$N$がもっと小さければ単純に再帰書いて文字列作っていけばいいけど, $N$が$50$まで行くから無理(最低$2^{50}$文字).
徐々に問題のサイズを小さくしていけば良さそう.

以下, レベル$L$のときのバーガーの層の総数を$l[L]$, `P`の個数を$p[L]$とする.

$N=2$のとき, {{< colorize "#ff0000" "B" >}}{{< colorize "#0000cc" "BPPPB" >}}{{< colorize "#ff0000" "P" >}}{{< colorize "#0000cc" "BPPPB" >}}{{< colorize "#ff0000" "B" >}}.

- $X$が赤い文字に対応するときは, バーガーを完全に包含してるか, もしくは完全に含んでいないときなので, 再帰を終了.
- $X$が青い文字に対応するときは, レベル$N-1$のバーガーの途中だから, レベルを 1 つ下げて再帰.

こんな感じのイメージで再帰を回す.

### 実装

$X$は予め 1 引いて 0 から始まるようにした(なんかそっちのほうが単純そうだったので).

レベル$N$のとき, 赤い文字に対応するっていうことは, $X$は$0$, $\displaystyle\frac{l[N]}{2}$, $l[N]-1$.

- $X = 0$のとき, 何も含んでないので何もせずに終了.
- $\displaystyle X = \frac{l[N]}{2}$のとき, レベル$N-1$のバーガーを 1 つ含んでいるので, `res += p[N - 1] + 1`をして終了(この`+1`は中心の`P`).
- $X = l[N]-1$のとき, レベル$N$のバーガーを 1 つ含んでいるので, `res += p[N]`をして終了.

青い文字に対応するっていうことは, $\displaystyle X > \frac{l[N]}{2}$ と $\displaystyle X < \frac{l[N]}{2}$.

- $\displaystyle X > \frac{l[N]}{2}$のとき, レベル$N-1$のバーガーを 1 つ含んでいるため, まずそれを上の$\displaystyle X=\frac{l[N]}{2}$みたいに足して, `X -= l[N - 1] + 2` (この括弧内の分: [{{< colorize "#ff0000" "B" >}}{{< colorize "#0000cc" "BPPPB" >}}{{< colorize "#ff0000" "P" >}}]{{< colorize "#0000cc" "BPPPB" >}}{{< colorize "#ff0000" "B" >}}).
- $\displaystyle X < \frac{l[N]}{2}$のとき, 何も含んでいないので何もせずに, `X -= 1` (頭の`B`の分).

レベル 0 になったら, もうそれ以上分けられないから, `res += 1`して終了.

## [提出コード(Python3:snake:)](https://atcoder.jp/contests/abc115/submissions/3748369)

```python
n,x = map(int,input().split())
x -= 1

p = [1]
l = [1]
for i in range(n):
    p.append(p[-1] * 2 + 1)
    l.append(l[-1] * 2 + 3)

res = 0
while n >= 0:
    if n == 0:                 # レベル0のバーガーはこれ以上細分化できない
        res += 1
        break
    elif x == 0:               # 左端
        break
    elif x == l[n] - 1:        # 右端
        res += p[n]
        break
    elif x == l[n] // 2:       # 中心
        res += p[n - 1] + 1
        break
    elif x > l[n] // 2:        # 1個包含している
        res += p[n - 1] + 1
        x -= l[n - 1] + 2
        n -= 1
    else:                      # 包含していない
        n -= 1
        x -= 1

print(res)
```
