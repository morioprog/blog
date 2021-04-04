---
title: "【ABC043】D - アンバランス / Unbalanced"
date: 2018-08-22T00:00:00+09:00
thumbnail: "img/header/atcoder.svg"
categories: ["精進"]
tags: ["文字列"]
---

## [問題概要](https://atcoder.jp/contests/abc043/tasks/arc059_b)

文字列$s$の中に, 過半数が同じ文字となる 2 文字以上の部分文字列$t$(アンバランスな部分文字列)は存在するか判定せよ.

$t$が存在するならその区間を, しないなら`-1 -1`を出力せよ.

### 制約

- $2 \leq |s| \leq 10^5$
- $s$は小文字のアルファベットのみからなる.

## 考察

$t$は 2 文字以上なので, とりあえず順番にどんな文字列がアンバランスなのか考えてみる.

2 文字の場合は,

- `aa`や`pp`といった**AA 型**

以上, 1 通りのみ.

3 文字の場合は,

- `dad`や`mom`といった**ABA 型**
- `zoo`や`see`といった**ABB 型**
- `ddr`や`mmo`といった**AAB 型**
- `aaa`や`zzz`といった**AAA 型**

以上, 4 通りだが, 内 ABB 型, AAB 型, AAA 型は AA 型を含んでいるため, 実際は**ABA 型**のみを考えればよさそう.

4 文字の場合は,

- `aaaa`や`zzzz`といった**AAAA 型**
- `aaaz`や`zzza`といった**AAAB 型**
- `aaza`や`zzaz`といった**AABA 型**
- `azaa`や`zazz`といった**ABAA 型**
- `zaaa`や`azzz`といった**BAAA 型**

以上, 5 通りだが, 全て AA 型を含んでいるため考えなくてよさそう.

同様に 5 文字以降も考えていくと, **AA 型**か**ABA 型**を含んでいそうなので, この 2 通りを判別する.

## [提出コード(Python3:snake:)](https://atcoder.jp/contests/abc043/submissions/3013859)

```python
s = input()
for i in range(len(s) - 1):
    if s[i] == s[i + 1]:
        print(i + 1, i + 2)
        exit()
for i in range(len(s) - 2):
    if s[i] == s[i + 2]:
        print(i + 1, i + 3)
        exit()
print(-1, -1)
```
