---
title: "dcゴルフの備忘録"
date: 2020-04-20T03:36:07+09:00
categories: ["備忘録", "コードゴルフ"]
tags: ["dc"]
---

## はじめに

`man dc`を個人的にまとめたもの. (間違ってるかも)

基本的に AtCoder の dc 環境(v1.4.1)で検証をしています. 便宜上, スタックの top から順に, $s_1, s_2, \ldots$と定義します.

## 入力

`?`で 1 行入力を受け取る. 入力された内容を[マクロ](#macro)として実行する.

例) `4 2 9`を入力 → 4 をスタックに push → 2 をスタックに push → 9 をスタックに push

## 出力

| コマンド | 処理                                                                         | 改行 |
| :------: | :--------------------------------------------------------------------------- | :--: |
|   `p`    | $s_1$を出力                                                                  | あり |
|   `n`    | $s_1$を出力して pop                                                          | なし |
|   `P`    | $s_1$を文字列として出力して pop (数値なら 256 進数にして ASCII コードに変換) | なし |
|   `f`    | スタック全体を出力 (上が top)                                                | あり |

## 数値

整数・小数をスタックに push したいときは単にその数値を書く.

- 複数の値を連続して push したいときは各数値の間に空白を開ける.
  - `17 9.2` : $17$と$9.2$を push
  - 一部コマンドで代替することで空白を省ける[ケース](#tech_10)もある.
    - `K` : `0`
    - `O` : `10`
- **負数を表すマイナスは`_`を用いる**.
  - `_28` : `-28`を push
- $0,1,\ldots,9,A(10),B(11),\ldots,F(15)$を, **$i$進数($i$は[入力基数](#param))の各桁の値**として用いる.
  - `429` : $4 \cdot i^2 + 2 \cdot i^1 + 9 \cdot i^0$
  - `23.7` : $2 \cdot i^1 + 3 \cdot i^0 + 7 \cdot i^{-1}$
  - `_B7E` : $-(11 \cdot i^2 + 7 \cdot i^1 + 14 \cdot i^0)$
  - `BE.D` : $11 \cdot i^1 + 14 \cdot i^0 + 13 \cdot i^{-1}$
  - yukicoder の dc 環境(v1.3.95)では挙動が一部異なる
    - 1 桁の場合は同じように$0,1,\ldots,15$を表す.
    - 2 桁以上(小数点以下も含む)の場合は, **$i$以上の桁は$i - 1$に抑えられる**. ($i$進数の各桁は本来$0$から$i - 1$であるべきなので)
      - `_A7E` : $-(9 \cdot 10^2 + 7 \cdot 10^1 + 9 \cdot 10^0)$ ($i = 10$のとき)

## 演算

**演算に使用した値は(基本的に)pop される**. 精度は一部例外を除いて被演算子で定まる.

| コマンド | 処理                                                     | 注意                                                                               |
| :------: | :------------------------------------------------------- | :--------------------------------------------------------------------------------- | ---------------------------------------------------------- |
|   `+`    | $s_2 + s_1$を積む                                        | -                                                                                  |
|   `-`    | $s_2 - s_1$を積む                                        | -                                                                                  |
|   `*`    | $s_2 \times s_1$を積む                                   | -                                                                                  |
|   `/`    | $s_2 \div s_1$を積む                                     | $s_1=0$なら何も起こらない(**pop もされない**). 精度は[精度値](#param)で指定される. |
|   `%`    | $s_2 \ \mathrm{mod}\  s_1$を積む                         | $s_1=0$なら何も起こらない(**pop もされない**).                                     |
|   `~`    | $s_2 \div s_1$と$s_2 \ \mathrm{mod}\  s_1$をこの順に積む | $s_1=0$なら何も起こらない(**pop もされない**). 精度は[精度値](#param)で指定される. |
|   `^`    | $s_2 ^ {s_1}$を積む                                      | $s_1$の小数点以下は無視される.                                                     |
|    `     | `                                                        | ${s_3 ^ {s_2}} \ \mathrm{mod}\  s_1$を積む                                         | $s_1=0$なら何も起こらない(**pop もされない**). 整数を渡す. |
|   `v`    | $\sqrt{s_1}$を積む                                       | $s_1<0$なら pop のみ行われる.                                                      |

## スタック操作

| コマンド | 処理                                                                                                                                                                               |
| :------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   `c`    | スタックを空にする                                                                                                                                                                 |
|   `d`    | $s_1$を積む                                                                                                                                                                        |
|   `r`    | $s_1$と$s_2$を swap する                                                                                                                                                           |
|   `R`    | $s_1$を pop して, スタックの上部$\lvert s_1 \rvert$個を rotate する.<br>$\cdot\ s_1$が正なら, 元の$s_2$が$s_3$に動く方向へ.<br>$\cdot\ s_1$が負なら, 元の$s_3$が$s_2$に動く方向へ. |

## レジスタ操作

操作するレジスタ名を$(reg)$として表す. レジスタ名は 1 文字で指定する.

各レジスタは**それ自身のスタック**を持っている.

|  コマンド  | 処理                                                    |
| :--------: | :------------------------------------------------------ |
| `s`$(reg)$ | $s_1$を pop し, $(reg)$に保存する($\neq$スタックに積む) |
| `l`$(reg)$ | $(reg)$の top の内容をスタックに積む                    |
| `S`$(reg)$ | $s_1$を pop し, $(reg)$に積む                           |
| `L`$(reg)$ | $(reg)$の top を**pop して**スタックに積む              |

<a id="param"></a>

## パラメータ

- 精度 : 前述した一部演算を行う際の精度. 0 以上の値.
- 入力基数 : 何進数として入力するか. 2 以上 16 以下の値.
- 出力基数 : 何進数として出力するか. 2 以上の値.

| コマンド | 処理                            |
| :------: | :------------------------------ |
|   `k`    | $s_1$を pop し, 精度に設定.     |
|   `i`    | $s_1$を pop し, 入力基数に設定. |
|   `o`    | $s_1$を pop し, 出力基数に設定. |
|   `K`    | 現在の精度をスタックに積む.     |
|   `I`    | 現在の入力基数をスタックに積む. |
|   `O`    | 現在の出力基数をスタックに積む. |

<a id="macro"></a>

## 文字列 / マクロ

単に表示する, もしくはマクロ(dc の命令列)として実行することができる.

操作するレジスタ名を$(reg)$として表す.

|  コマンド   | 処理                                                                                                                                 |
| :---------: | :----------------------------------------------------------------------------------------------------------------------------------- |
|  `[hoge]`   | 文字列`hoge`をスタックに積む.                                                                                                        |
|     `a`     | $s_1$を pop し, 数値なら$s_1 \ \mathrm{mod}\  256$を ASCII コードとして文字列にしたものを, 文字列ならその最初の文字をスタックに積む. |
|     `x`     | $s_1$を pop し, マクロとして実行する.                                                                                                |
| `>`$(reg)$  | $s_1 > s_2$なら, $(reg)$の内容をマクロとして実行する.                                                                                |
| `!>`$(reg)$ | $s_1 \leq s_2$なら, $(reg)$の内容をマクロとして実行する.                                                                             |
| `<`$(reg)$  | $s_1 < s_2$なら, $(reg)$の内容をマクロとして実行する.                                                                                |
| `!<`$(reg)$ | $s_1 \geq s_2$なら, $(reg)$の内容をマクロとして実行する.                                                                             |
| `=`$(reg)$  | $s_1 = s_2$なら, $(reg)$の内容をマクロとして実行する.                                                                                |
| `!=`$(reg)$ | $s_1 \neq s_2$なら, $(reg)$の内容をマクロとして実行する.                                                                             |
|     `q`     | 実行中のマクロと, それを呼び出したマクロを終了する. `q`を含むマクロが最上層で行われた場合は exit する.                               |
|     `Q`     | $s_1$を pop し, その回数だけマクロから抜ける. 最上層に到達しても exit しない.                                                        |

## その他

|    コマンド    | 処理                                                                                |
| :------------: | :---------------------------------------------------------------------------------- |
|      `Z`       | $s_1$を pop し, それが文字列ならその長さを, そうでないならその桁数をスタックに積む. |
|      `X`       | $s_1$を pop し, それが文字列なら$0$を, そうでないならその精度をスタックに積む.      |
|      `z`       | 現在のスタックのサイズをスタックに積む.                                             |
| `!`$(command)$ | $(command)$をシステムコマンドとして実行する.                                        |
|      `#`       | 続く文字列はコメントとして処理される.                                               |
|   `:`$(arr)$   | $s_1, s_2$を pop し, 配列$(arr)$の$s_1$番目に$s_2$を保存する.                       |
|   `;`$(arr)$   | $s_1$を pop し, 配列$(arr)$の$s_1$番目の値をスタックに積む.                         |

## ゴルフテク

- `k`,`i`で疑似 pop
  - 精度と(入力後の)入力基数は変えても問題ないことが多いので.
- `X`で top を 0 に
  - top が整数であり不要なら`X`で 0 にできる. (その後は`+`して消すなり, ループで適当に処理するなり.)
- `[neg] [pos] [x]`で`vrp`
  - $x < 0$なら`[neg]`, そうでないなら`[pos]`を出力.
  - `v`に負値を渡すと pop のみ行われることを利用.
    <a id="tech_10"></a>
- 単に$10$を push したいときは`O`,`I`を使う
  - これを利用して`O9^7+`で$10^9+7$が表せる.
    - `O9` : `O`は桁じゃないので, $10$と$9$のように独立の数としてパースされる
    - `A9` : `A`は桁なので, $10 \cdot 10 + 9 = 109$としてパースされる
- `s`で上書きしていないレジスタにアクセスすると, スタックに$0$が積まれる.
  - 「ある条件を満たしたら$0$を積む」みたいなのを短く書ける.
    - 例. [ABC164A - Sheep and Wolves](https://atcoder.jp/contests/abc164/submissions/12403067)
- `P`で$0$や$10$を(実質)空文字扱いできる
  - $0$ : ヌル文字
  - $10$ : 改行文字
