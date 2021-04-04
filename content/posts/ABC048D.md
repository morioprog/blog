---
title: "【ABC048】D - An Ordinary Game"
date: 2018-11-02T00:00:00+09:00
thumbnail: "img/header/atcoder.svg"
categories: ["精進"]
tags: ["文字列"]
---

## [問題概要](https://atcoder.jp/contests/abc048/tasks/arc064_b)

操作後に同一の文字が隣り合わないように, 両端以外からお互いに 1 文字ずつ取り除いていく.

このような操作を行うことが出来なくなった方が負けとなる. 先手と後手のどっちが勝つかを出力せよ.

### 制約

- $3\leq |s|\leq 10^{5}$
- $s$の中に同一の文字が隣り合う箇所はない

## 考察

操作できなくなる寸前の文字列の長さの偶奇が分かれば, 初期状態の文字列の長さの偶奇と比べることによりどっちが勝つか分かりそう.

両端の文字は絶対取り除けないから, 一旦$s$からそれらを抜き出してみる.

例えば, `adbcadbcb` は `ababb`になる. 制約より, この隣り合っている`b`の間に何らかの文字があったことは分かるので, `abab?b`となる.

初期状態では 9 文字だったのが, 操作できなくなる寸前では 6 文字と, 計 3 回の操作を行った時点で詰んでいるため先手の勝ちだと分かる.

厳密な証明が出来ないのが怖いけど, 通ったからよし(よくない).

## [提出コード(Python3:snake:)](https://atcoder.jp/contests/abc048/submissions/3517034)

```python
s = input()
t = [i for i in s if i in (s[0], s[-1])]
r = len(t)
for c, d in zip(t[:-1], t[1:]):
    r += (c == d)
print('Second' if len(s) % 2 == r % 2 else 'First')
```

### 蛇足

(解説が天才すぎて理解できない；。；)
