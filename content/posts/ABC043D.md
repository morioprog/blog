---
title: "【ABC043】D - アンバランス / Unbalanced"
date: 2018-08-22T00:00:00+09:00
categories: ["精進"]
tags: ["文字列"]
---

## [問題概要](https://atcoder.jp/contests/abc043/tasks/arc059_b)

文字列$s$の中に, 過半数が同じ文字となる2文字以上の部分文字列$t$(アンバランスな部分文字列)は存在するか判定せよ.

$t$が存在するならその区間を, しないなら`-1 -1`を出力せよ.

### 制約

* $2 \leq |s| \leq 10^5$
* $s$は小文字のアルファベットのみからなる.

## 考察

$t$は2文字以上なので, とりあえず順番にどんな文字列がアンバランスなのか考えてみる.

2文字の場合は, 

* `aa`や`pp`といった**AA型**

以上, 1通りのみ.

3文字の場合は, 

* `dad`や`mom`といった**ABA型**
* `zoo`や`see`といった**ABB型**
* `ddr`や`mmo`といった**AAB型**
* `aaa`や`zzz`といった**AAA型**

以上, 4通りだが, 内ABB型, AAB型, AAA型はAA型を含んでいるため, 実際は**ABA型**のみを考えればよさそう.

4文字の場合は, 

* `aaaa`や`zzzz`といった**AAAA型**
* `aaaz`や`zzza`といった**AAAB型**
* `aaza`や`zzaz`といった**AABA型**
* `azaa`や`zazz`といった**ABAA型**
* `zaaa`や`azzz`といった**BAAA型**

以上, 5通りだが, 全てAA型を含んでいるため考えなくてよさそう.

同様に5文字以降も考えていくと, **AA型**か**ABA型**を含んでいそうなので, この2通りを判別する.

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
