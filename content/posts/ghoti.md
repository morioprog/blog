---
title: "ぷよぷよAI（ghoti）の説明"
date: 2023-01-22T00:00:00+09:00
thumbnail: "img/header/ghoti.png"
categories: ["ぷよぷよAI"]
---

まだまだ実装したいことが山積みだけど、自分用のメモを兼ねて

## "ghoti" とは

2022年2月ぐらいに作り始めた**ぷよぷよAI**です

"ghoti" は **「ふぃっしゅ」** と読みます
（[Wikipedia - Ghoti](https://ja.wikipedia.org/wiki/Ghoti)）

{{< linebreak >}}

現状はSteamのぷよスポでのみ動作します（いずれはぷよテトでも動かしてみたい）

動作している様子はこんな感じ（1P: 僕、2P: ghoti）

{{< youtube hr0YxksDlKQ >}}

### アルゴリズム

操作部分の実装にあまりにも時間を掛けてしまったので、
現状はまだ簡単に実装できるところしか手を付けられていないです

一応案はいくつかあるので時間があるときに実装したいな...

- **モンテカルロ法 + ビームサーチ**
  - モンテカルロ部分は20並列
  - 探索速度はかなり遅い
  - 評価関数はGAでチューニングしてみてるけど、あんま上手くいってなさそう
  - モンテカルロ法の偉大な既存研究 → [SlideShare - ぷよぷよAIの新しい探索法](https://www.slideshare.net/takapt0226/ai-52214222)
- 凝視に基づいた発火判断
  - 本線のサイズとか地形とかを見てる
  - 正直、探索部分が雑魚でもこっちが強ければそこそこ戦える気がしているので強くしたいが...

## 実装部分

前々から使ってみたかったこともあり、全部 **Rust** で書いてみてます

初学者が適当に書いたコードなのでまさかり大歓迎です！！

ちなみにゲーム内操作の部分は公開していません（なんかこわいので）

{{< github morioprog ghoti >}}

### 大まかな構造

肝心なのはここらへん

- `cpu`: AIの思考部分
  - [`cpu/src/bot`](https://github.com/morioprog/ghoti/tree/main/cpu/src/bot) にいろいろ置いてあります
- `simulator`: とこぷよ/2P対戦のシミュレータ
  - フレーム数とかはぷよスポ準拠にしようとしてるけどバグってそう
- `optimizer`: 評価関数のチューニング

### ちなみに...

テトリスAIのColdClearをめちゃくちゃ~~パクって~~参考にしています

実装がキレイだしちゃんと強くてすごい

{{< github MinusKelvin cold-clear >}}

{{< linebreak >}}

どうやら書き直したバージョンもあるみたいですが追えてません

{{< github MinusKelvin cold-clear-2 >}}

## おわりに

ぷよAIたのしいのでみんなもやろう！

もし何か知りたいことがあったら [Twitter](https://twitter.com/morio_prog) までお願いします！:bow:
