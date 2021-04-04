---
title: "M1 Macの初期設定"
date: 2021-02-18T00:00:00+09:00
thumbnail: "img/header/m1.jpeg"
categories: ["備忘録"]
---

## システム環境設定

- トラックパッド
  - 「調べる＆データ検出」を「3 本指でタップ」に
  - 「タップでクリック」を付ける
  - 「クリック」を「弱い」に
  - 「軌跡の速さ」を「速い」に
- キーボード
  - Caps Lock を Esc に
  - 地球儀キーを Command に
  - 「ライブ変換」をオフに
  - 「タイプミスを修正」をオフに
- Mission Control
  - 「最新の使用状況に...」をオフに
- Dock とメニューバー
  - Wifi を非表示
  - バッテリーを非表示
    - Battery Buddy を使ってみたかったので
  - Spotlight を非表示
  - Siri を非表示
  - 時計を 24 時間表示に
- ディスプレイ
  - 解像度を変更してスペースを拡大
- キーボード
  - キーのリピートを最速に
  - リピート入力認識までの時間を最短に
- アクセシビリティ
  - ディスプレイ ＞ 「視差効果を減らす」

## アプリ

- [Typora](https://typora.io)
- [Brave Browser](https://brave.com/ja/download/)

  - 設定 ＞ キーボード ＞ ショートカット ＞ アプリケーション
    - 「ページを別名で保存...」を「Option + Cmd + S」に
    - 「Brave を終了」を「Shift + Cmd + Q」に
  - [Skip Silence](https://chrome.google.com/webstore/detail/skip-silence/fhdmkhbefcbhakffdihhceaklaigdllh/related)
  - [Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb?hl=ja)
    - [ナビゲーションバーからフォーカスを外す](https://qiita.com/embokoir/items/603e55ecfb515ed6c378#%E3%81%AA%E3%82%84%E3%81%BF)

- [VSCode](https://code.visualstudio.com/insiders/#) : ARM64 版
  - minimap を非表示に
  - Vim の拡張機能
    - [【VSCode】エディタ、サイドバー、ターミナル間のフォーカスのショートカットを設定する](https://qiita.com/m1ul24/items/c0a53885b893f121082f)
    - 長押しで連打判定されないのを直す → [■](https://taupe.site/entry/vscode/#VSCode%25E3%2581%25AEVim%25E3%2581%25A7%25E3%2582%25AD%25E3%2583%25BC%25E9%2595%25B7%25E6%258A%25BC%25E3%2581%2597%25E3%2581%25A7%25E3%2581%25AE%25E7%25A7%25BB%25E5%258B%2595%25E3%2581%258C%25E3%2581%25A7%25E3%2581%258D%25E3%2581%25AA%25E3%2581%2584%25E3%2583%2590%25E3%2582%25B0%25EF%25BC%2588Mac%25EF%25BC%2589)
      ```shell
      $ defaults write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false
      ```
- [Magnet](https://apps.apple.com/jp/app/magnet-%E3%83%9E%E3%82%B0%E3%83%8D%E3%83%83%E3%83%88/id441258766?mt=12)
- [TickTick](https://apps.apple.com/jp/app/ticktick-things-tasks-to-do/id966085870?mt=12)
- [iTerm2](https://iterm2.com/)
  - [iTerm2 のおすすめ設定〜ターミナル作業を効率化する〜](https://qiita.com/ruwatana/items/8d9c174250061721ad11)
  - [iTerm2 を HotKey で起動した時にフルスクリーンで重ねて表示する](https://www.smartbowwow.com/2018/05/iterm2hotkey.html)
- [RunCat](https://apps.apple.com/jp/app/runcat/id1429033973?mt=12)
- [Alfred](https://www.alfredapp.com/)
  - [Alfred を使いこなせてない君に！【Alfred の使い方完全版】](https://qiita.com/jackchuka/items/ccd3f66f6dd00481b98b)
- [Battery Buddy](https://batterybuddy.app/[)

### 未対応（Rosetta 2）

- [Biscuit](https://eatbiscuit.com/ja)
- [Bitwarden](https://bitwarden.com/download/)
- [Logi Options](https://www.logicool.co.jp/ja-jp/product/options)
- [Sublime Text](https://www.sublimetext.com/)

## anyenv

事前に`brew`を導入した → [■](https://zenn.dev/ress/articles/069baf1c305523dfca3d)

- [anyenv/anyenv](https://github.com/anyenv/anyenv)
- [anyenv と nodenv を使った Node.js の環境構築](https://qiita.com/kenji7157/items/e0e07f96a972f1c7fb8c)
- [Mac 上で anyenv の nodenv で管理している npm で入れたモジュールにパスを通す](https://qiita.com/USI/items/20c713ca6b9e5b8b2bea)
  - コメントを参照

```shell
=brew install anyenv
anyenv install --init
echo 'eval "$(anyenv init -)"' >> ~/.zshrc
exec $SHELL -l
anyenv install nodenv
nodenv install -l
nodenv install 15.7.0
nodenv global 15.7.0
npm install -g yarn
nodenv rehash   # コマンドが使えるようになる
```

## Vim

### dein.vim

- [dein.vim のインストール自体にハマってしまったメモ](https://qiita.com/Coolucky/items/0a96910f13586d635dc0)
- [vim-devicons で格好いい vim を作ろう。](https://qiita.com/park-jh/items/4358d2d33a78ec0a2b5c>)
- iTerm2 の「Use a different font for non-ASCII characters」をオンにして、そこに Droid Sans Mono を追加する

```sh
curl https://raw.githubusercontent.com/Shougo/dein.vim/master/bin/installer.sh > installer.sh
# For example, we just use `~/.cache/dein` as installation directory
sh ./installer.sh ~/.cache/dein

# Nerd Fontの導入
cd ~/Library/Fonts && curl -fLo "Droid Sans Mono for Powerline Nerd Font Complete.otf" https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/DroidSansMono/complete/Droid%20Sans%20Mono%20Nerd%20Font%20Complete.otf
```

## その他やったこと

- zsh に prezto を入れた → [■](https://github.com/sorin-ionescu/prezto), [■](https://github.com/sindresorhus/pure)
- Themer でテーマを作ってみた → [■](https://themer.dev/?colors.dark.shade0=%2326363a&colors.dark.shade7=%23e7e7de&colors.dark.accent0=%23eb596e&colors.dark.accent1=%23487e95&colors.dark.accent2=%23f8dc81&colors.dark.accent3=%2316c79a&colors.dark.accent4=%2351c2d5&colors.dark.accent5=%23a2d0c1&colors.dark.accent7=%23f88f01&colors.dark.accent6=%23f7f7e8&activeColorSet=dark)
  - （結局 Iceberg にしたけど）
- uBlacklist の設定 → [■](https://github.com/arosh/ublacklist-stackoverflow-translation)
- 競プロ関連の UserScript の追加
  - [ac-predictor](https://greasyfork.org/ja/scripts/369954-ac-predictor)
  - [Time Limit Emphasizer](https://greasyfork.org/ja/scripts/406381-time-limit-emphasizer)
  - [AtCoderResultNotifier](https://greasyfork.org/ja/scripts/371225-atcoderresultnotifier)
  - [AtCoderStandingsAnalysis](https://greasyfork.org/ja/scripts/398439-atcoderstandingsanalysis)
  - [AtCoder dos2unix UserScript](https://greasyfork.org/ja/scripts/372122-atcoder-dos2unix-userscript)
  - [atcoder-tasks-page-colorizer](https://greasyfork.org/ja/scripts/380404-atcoder-tasks-page-colorizer)
  - [AtCoderPerformanceColorizer](https://greasyfork.org/ja/scripts/371693-atcoderperformancecolorizer)
  - [AtCoder Submission User Colorizer](https://greasyfork.org/ja/scripts/397710-atcoder-submission-user-colorizer)
  - [AtCoder Language Filter](https://greasyfork.org/ja/scripts/398148-atcoder-language-filter)
  - [Add Shortest Tab](https://greasyfork.org/ja/scripts/391692-add-shortest-tab)
