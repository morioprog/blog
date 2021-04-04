---
title: "【ABC114】C - 755"
date: 2018-12-02T00:00:00+09:00
thumbnail: "img/header/atcoder.svg"
categories: ["精進"]
tags: ["全探索", "bit全探索", "DFS", "桁DP", "埋め込み"]
---

## [問題概要](https://atcoder.jp/contests/abc114/tasks/abc114_c)

$1$以上$N$以下の整数のうち, `7`, `5`, `3`が 1 回以上現れ, なおかつそれら以外の数字が現れないものの個数を求めよ.

### 制約

- $1 \leq N < 10^9$

## 考察

**bit 全探索**的なノリで数えれば良さそう.
とりあえず 3 進法で書いてみたけど, 頭の`0`に対応するのめんどくさそうだったから 4 進法に書き直した.

### おおまかな工程

ループ変数は$i$.

1. $i$を 4 進数に変換する(`tobase4`).
1. それが`1`, `2`, `3`を含んでおり, かつ`0`を含んでいなければ残りの処理を続行(`check`).
1. `1`を`3`, `2`を`5`, `3`を`7`に変えたとき(変換の順序に注意), それが$N$より大きいなら終了. そうでないなら`res++`.

## [提出コード(Python3:snake:)](https://atcoder.jp/contests/abc114/submissions/3705392)

```python
n = int(input())

def tobase4(num):
    ans = ''
    while num>0:
        ans += str(num%4)
        num //= 4
    return ans[::-1]

def check(s):
    if '1' in s and '2' in s and '3' in s and '0' not in s:
        return True
    return False

def conv(s):
    s = s.replace('2','5')
    s = s.replace('3','7')
    s = s.replace('1','3')
    return s

res = 0
for i in range(10**9):
    s = tobase4(i)
    if not check(s):
        continue
    s = int(conv(s))
    if n < s:
        break
    res += 1
print(res)
```

### 別解

1. [DFS(解説はこれ)](https://atcoder.jp/contests/abc114/submissions/3710401)
2. [`itertools.product`](https://atcoder.jp/contests/abc114/submissions/3712651)
3. 桁 DP
4. †埋め込み†
