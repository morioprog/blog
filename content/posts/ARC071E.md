---
title: "【ARC071】E - TrBBnsformBBtion"
date: 2018-12-06T00:00:00+09:00
thumbnail: "img/header/atcoder.svg"
categories: ["精進"]
tags: ["文字列", "imos法"]
---

## [問題概要](https://atcoder.jp/contests/arc071/tasks/arc071_c)

`A`, `B`のみからなる文字列$S$, $T$が与えられる.

$S$の部分文字列$s$について, 以下の 4 つの操作を好きなだけ行ってよいとき, $T$の部分文字列$t$に変換することは出来るかという判定クエリに$q$個答えなさい.

1. `A`を`BB`にする.
1. `B`を`AA`にする.
1. `AAA`を消す.
1. `BBB`を消す.

### 制約

- $1 \leq |S|, |T|, q \leq 10^5$
- $s, t$はそれぞれ$S, T$の部分文字列.

## 考察

制約ををざっと見てみると, クエリの数が最大$10^5$個と多すぎる.

判定に使えるオーダーは高々$O(log|S|)$程度だけど, $\log$のオーダーで文字列処理するアルゴリズムなんてあるのか...?

$O(1)$臭が凄いから, 諸々証明出来るか試してみる.

### 仮説 1. 操作は可逆である

真だったら, それぞれの文字の個数を$\mathrm{mod}\ 3$で考えることにより, 操作 3,4 とその逆は考慮に入れる必要が無くなる.

- 操作 1, 2 について, `BB` → `AAAA` → `A`
- 操作 3, 4 については, 操作後に 1 文字は必ず残っているため, その 1 文字が`A`の場合と`B`の場合に分けて考える.

  - `A` → `BB` → `AAAA`
  - `B` → `AA` → `ABB` → `AAAB`
  - `B` → `AA` → `BBA` → `BAAA`

よって, 全ての操作が可逆であると分かった.

### 仮説 2. $s, t$の並びは結果に影響しない

真だったら, それぞれの文字の個数について考えるだけで良くなる. (偽だったら, $s, t$について走査しなきゃいけないから$O(1)$じゃない.)

以下便宜上, 文字`A`が$n$個並んだ文字列を`[A, n]`と表記する.

自然数$a, b$について, `[A, a] + [B, b]`になんらかの操作をすることにより`[B, 1] + [A, a] + [B, b - 1]`に変換できたなら, 各操作における A, B の対称性より, その操作を繰り返すと任意の並びにできる(はず).

実際に, 以下のように操作が行える.

1. `[A, a] + [B, b]`
1. `[A, a] + [B, 1] + [B, b - 1]`
1. `[A, a] + [A, 2] + [B, b - 1]`
1. `[A, 1] + [A, 1] + [A, a] + [B, b - 1]`
1. `[B, 2] + [B, 2] + [A, a] + [B, b - 1]`
1. `[B, 3] + [B, 1] + [A, a] + [B, b - 1]`
1. `[B, 1] + [A, a] + [B, b - 1]`

仮説 1 より, この逆の操作も行えるため, 示された(と思う).

### 最後の詰め

先ほど述べたように, 操作 3, 4 とその逆は考える必要がなくなった(操作 1,2 を行って, 最後に差分が 3 の倍数であれば変換可能と分かるため).

$s$の中の`A`, `B`の個数をそれぞれ$a, b$個, $t$の中のそれらの個数をそれぞれ$a + m, b + n$個とする.

今, 操作 1 を$p$回, 操作 2 を$q$回行うと, `A`は$q - p$個, `B`は$p - q$個増えるため, $a + q - p, b + p - q$個となる.

よって, $a + m \equiv a + q - p,\ b + n \equiv b + p - q\ (\mathrm{mod}\ 3)$を満たしていれば良い.

ちょっと式変形して$-m\equiv n(\mathrm{mod}\ 3)$ならば`YES`, そうでないならば`NO`となる.

### 実装

imos 法で任意区間における`A`の個数を$O(1)$で取り出せるようにしておいて, あとは上の判定式に突っ込んで出力するだけ.

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/arc071/submissions/3727188)

```cpp
string S, T;
int Q;

ll umod(ll a) {
    ll ret = a % 3;
    if (ret < 0) return ret + 3;
    return ret;
}
signed main() {
    cin >> S >> T;
    int len_s = S.size();
    int len_t = T.size();

    CumulativeSum<int> S_A(len_s), T_A(len_t);
    REP(i, len_s) if (S[i] == 'A') S_A.add(i, 1);
    REP(i, len_t) if (T[i] == 'A') T_A.add(i, 1);
    S_A.build();
    T_A.build();

    int a, b, c, d;
    cin >> Q;
    while (Q--) {
        cin >> a >> b >> c >> d;
        --a, --b, --c, --d;
        int s_a = (int)S_A.query(a, b), s_b = (b - a + 1) - s_a;
        int t_a = (int)T_A.query(c, d), t_b = (d - c + 1) - t_a;
        int dif_a = t_a - s_a;
        int dif_b = t_b - s_b;
        cout << ((umod(dif_a) == umod(dif_b)) ? "YES" : "NO") << endl;
    }
}
```
