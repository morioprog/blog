---
title: "Windowsの初期設定"
date: 2022-02-13T00:00:00+09:00
thumbnail: "img/header/m1.jpeg"
categories: ["備忘録"]
---

## WSL2 の導入

（記憶が曖昧なので、順番前後してそう）

1. BIOS から CPU 仮想化の設定を ON にする。
1. 「設定 > アプリ > オプション機能 > Windows のその他の機能」から、「仮想マシン プラットフォーム」と「Linux 用 Windows サブシステム」を有効化する。
1. コマンドプロンプトから下のコマンドを叩いて、WSL をインストールする。
   ```console
   $ wsl --install
   ```
1. このパッチを当てる。
   - <https://docs.microsoft.com/ja-jp/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package>
1. PowerShell からデフォルトの WSL のバージョンを 2 にする。
   ```console
   $ wsl --set-default-version 2
   ```
1. Microsoft Store からここら辺をダウンロードする。
   - Ubuntu 20.04
   - PowerShell 2
   - Windows Terminal
1. Ubuntu を起動すると WSL2 になってるはず
   - `wsl --list --verbose`を叩くと確認できる

## 諸々の導入

### Homebrew

[公式ドキュメント](https://brew.sh/index_ja)に従って導入した。
とはいってもこれを叩いただけ。
叩いた後の出力に、次に叩くべきコマンドが書いてあるはずなのでそれに従った。

```console
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

`brew`を使ってここらへんを入れた。

- fish

  ```console
  $ brew install fish
  $ sudo sh -c "echo $(which fish) >> /etc/shells"
  $ chsh -s $(which fish)
  ```

  - ついでに[fisher](https://github.com/jorgebucaran/fisher)を入れて、pure を入れた

    ```console
    $ curl -sL https://git.io/fisher | source && fisher install jorgebucaran/fisher
    $ fisher install rafaelrinaldi/pure
    ```

- fzf
  ```console
  $ brew install fzf
  $ /home/linuxbrew/.linuxbrew/opt/fzf/install
  ```
- asdf
  ```console
  $ brew install asdf
  $ echo -e "\nsource "(brew --prefix asdf)"/asdf.fish" >> ~/.config/fish/config.fish
  ```

### Docker

ここら辺の公式ドキュメントに従った。

- <https://docs.docker.com/engine/install/ubuntu/>
- <https://docs.docker.com/engine/install/linux-postinstall/>
- <https://docs.docker.com/compose/install/>

```console
$ sudo apt-get update
$ sudo apt-get install ca-certificates curl gnupg lsb-release
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
$ sudo service docker start
$ sudo docker run hello-world   # 動くはず
```

このままだと`docker`使うたびに`sudo`が必要なので、docker グループを作った。

```console
$ sudo groupadd docker
$ sudo gpasswd -a $USER docker
$ sudo service docker stop
$ sudo service docker start
```

### キーボードの設定

基本的には Mac と同じ感じで使いたかったので、AutoHotKey でちょこちょこ変えた。
Windows キーはあんまつかわなさそうだったから、同じくあんま使わない Alt を片方拝借。
代わりにめっちゃつかう Ctrl を量産した。
あとは、なぜか 2 つあるバックスペースのキーの片方をアンダースコアに割り当てた。

```ahk
LWin::Ctrl
RWin::Ctrl
RAlt::LWin
sc073::_
```

### マウスの設定

仮想デスクトップ機能を有効活用するために、Logicool のマウスのボタンに諸々の機能を割り当てたかった。
が、Logicool Options からボタンに「タスクビュー」とかを割り当てたものの動かず...
キー割り当てとかでも同じ。

Logicool Options 側のボタンの押し方が下手なんだろうと推測して、AutoHotKey を経由することに。
具体的には下みたいな`.ahk`ファイルを書いて、使わないキー（というかキーボードにないキー）に各機能を割り当てた。

```ahk
AppsKey::#Tab
ScrollLock::#^Right
NumLock::#^Left
```

あとは Logicool Options から、ボタンを押したら各キーが押されるようにした結果動きました:tada:

### その他

- Windows Terminal から Ubuntu 20.04 を開いたときの開始ディレクトリの設定
  - ホームディレクトリになるようにした
    - `\\wsl$\Ubuntu-20.04\home\<username>`
- [SylphyHorn](https://www.microsoft.com/ja-jp/p/sylphyhorn/9nblggh58t01?activetab=pivot:overviewtab)で PIP を pin できるように
- `rm`で`trash-cli`が呼ばれるようにした
- VSCode のフォントファミリーを設定した: `'JetBrains Mono', 'M PLUS 2'`
- VSCode を半透明にする拡張機能: <https://marketplace.visualstudio.com/items?itemName=s-nlf-fh.glassit>
- 設定から拡張子を表示するようにした
  - 「設定 > プライバシーとセキュリティ > 開発者向け」から、使えそうな機能はだいたい有効にしておくとよさそう。
