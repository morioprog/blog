---
title: "コンテスト予定を表示するウィジェットの入れ方"
date: 2021-01-17T20:30:00+09:00
categories: ["その他"]
---

## どんなウィジェット？

この世に星の数ほどある**コンテストサイトの予定をまとめて表示するためのiOSウィジェット**です．

* 各コンテストをタップすると，そのコンテストサイトに飛びます
* `contestIds`からコンテストサイトを追加できます

どうやって導入するかの説明が不十分だったので，ここで改めてまとめておきます．

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">コンテスト予定を表示するウィジェットをつくりました🙌<br><br>Scriptableっていうアプリに下のコードを貼り付けて初期設定をすると動くはずです<br>各コンテストをタッチするとそのサイトに飛ぶので簡単にレジれたりもします<a href="https://t.co/etVUGOeZGI">https://t.co/etVUGOeZGI</a> <a href="https://t.co/L5VxJhtYNy">pic.twitter.com/L5VxJhtYNy</a></p>&mdash; もりを (@morio_prog) <a href="https://twitter.com/morio_prog/status/1327938750433828864?ref_src=twsrc%5Etfw">November 15, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## 導入方法

#### 1. 「Scriptable」をインストールする

まず，ウィジェットを動作させるためのアプリである，**Scriptable**をインストールします．

> Scriptable → [■](https://apps.apple.com/us/app/scriptable/id1405459188?uo=4)

#### 2. ソースコードをコピーする

ソースコードを**Gist**から持ってきます．
以下のリンクに飛んで，ソースコードをコピーしてください．
（Rawにした方がやりやすいかも）

> Gist → [■](https://gist.github.com/morioprog/c2cde4738678f10e561832ea14fd181b)

#### 3. Scriptableにソースコードを入れる

コピーできたら，Scriptableを起動します．
**右上の「＋」ボタン** からファイルを作成して，**左下の設定ボタン**を押してファイルの名前を変更しておきます．

変更できたら「Close」を押して，2枚目の状態に戻ったことを確認します．
何もないところを押すと入力モードになるので，そこにさっきコピーしたコードを貼り付けてください．

![手順3](/img/posts/widget/1.png)

#### 4. CLISTの設定

**CLIST**に行ってログインしてください．
新規登録も簡単にできます．

> CLIST → [■](https://clist.by/)

ログインできたらCLIST APIに飛んでください．

画面上部の **「show my api-key」** を押して，出てきたモーダルの下の方の **「Param query」** をコピーしてください．
（`/?username=...`みたいなやつです）

> CLIST API → [■](https://clist.by/api/v1/doc/)

Scriptableに戻り，ソースコード5行目の`CLIST_API`に先ほどコピーした「Param query」を貼ってください．

以上で下準備は終了です．

#### 5. ウィジェットの作成

ホーム画面に戻り，何もないところを長押しします．
左上の **細長い「＋」ボタン** を押すとウィジェット追加画面が出てくるので，その中からScriptableを選んでください．

ウィジェットの大きさを選んで， **「ウィジェットを追加」** を押すととりあえず無のウィジェットが生成できます．
ウィジェットの大きさですが，一番小さいもの以外は対応しているので自身のホーム画面と相談して選んでください．

生成されたウィジェットを長押しして **「ウィジェットを編集」** を押すと，下画像の2枚目になるので「Script」からさっき作ったソースコードを選択してください．

![手順5](/img/posts/widget/2.png)

#### 6. おわり！

お疲れ様でした．
これでウィジェットが表示されているはずです．

他のコンテストサイトをどうやって追加するかはまた書き加えます．

もし良ければGistをStarしてもらえると嬉しいです（小声）
