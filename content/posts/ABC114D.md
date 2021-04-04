---
title: "【ABC114】D - 756"
date: 2018-12-02T00:00:00+09:00
thumbnail: "img/header/atcoder.svg"
categories: ["精進"]
tags: ["数学", "約数"]
---

## [問題概要](https://atcoder.jp/contests/abc114/tasks/abc114_d)

$N!$の約数のうち, 約数をちょうど 75 個持つものの個数を求めよ.

### 制約

- $1 \leq N \leq 100$

## 考察

約数が 75 個ある整数を素因数分解したときどうなるか考えてみると,

- $a^2 \times b^4 \times c^4$
- $a^2 \times b^{24}$
- $a^4 \times b^{14}$
- $a^{74}$

の 4 通り. とりあえず$N!$の素因数がそれぞれ何個ずつあるのかを求めておいて, この 4 通りになりうるものが何個あるのかを足していけばいい.

指数が大きいものを決めてから、小さいものの帳尻を合わせれば数え漏れはないはず.

## [提出コード(C++:high_brightness:)](https://atcoder.jp/contests/abc114/submissions/3706086)

```cpp
map<int, int> prime_factor(int n) {
    map<int, int> ret;
    for (int i = 2; i * i <= n; i++) {
        while (n % i == 0) {
            ++ret[i];
            n /= i;
        }
    }
    if (n != 1) ret[n] = 1;
    return ret;
}

vector<int> d(101,0);
int c_2, c_4, c_14, c_24, c_74;

signed main() {
    int N;  cin >> N;

    for (int i = 1; i <= N; ++i) {
        auto p_f = prime_factor(i);
        for (auto itr = p_f.begin(); itr != p_f.end(); ++itr) {
            d[itr -> first] += itr -> second;
        }
    }

    for (auto& e : d) {
        if (e >=  2)  c_2++;
        if (e >=  4)  c_4++;
        if (e >= 14) c_14++;
        if (e >= 24) c_24++;
        if (e >= 74) c_74++;
    }

    int res = 0;
    // a^2 * b^4 * c^4 -> C(c_4,2) * (c_2 - 2)
    res += max(0, c_4 * (c_4 - 1) / 2 * (c_2 - 2));
    // a^2 * b^24 -> c_24 * (c_2 - 1)
    res += max(0, c_24 * (c_2 - 1));
    // a^4 * b^14 -> c_14 * (c_4 - 1)
    res += max(0, c_14 * (c_4 - 1));
    // a^74
    res += c_74;
    cout << res << endl;
}
```
