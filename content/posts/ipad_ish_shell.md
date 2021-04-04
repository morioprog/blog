---
title: "iPadの環境構築"
date: 2020-04-15T20:33:11+09:00
thumbnail: "img/header/ish.png"
categories: ["備忘録"]
---

## シェルアプリ

[iSH](https://ish.app/)という iOS 上で動作する Linux エミュレータを使います.

## 初期設定

### シェル

1. 必要な諸々をインストール

   ```console
   apk add zsh
   apk add vim
   apk add git
   apk add openssh
   apk add g++
   apk add python3
   ```

1. Vim などで`/etc/passwd`を以下のように編集して, デフォルトシェルを変更

   ```diff
   - root:X:0:0:root:/root:/bin/ash
   + root:X:0:0:root:/root:/bin/zsh
   ```

1. `oh-my-zsh`をインストール

   ```console
   wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh
   chmod u+x install.sh
   ./install.sh
   ```

1. `~/.zshrc`からテーマを変更

   ```diff
   - ZSH_THEME="robbyrussell"
   + ZSH_THEME="agnoster"
   ```

   - `vcs_info`でエラーが出るとき : `~/.oh-my-zsh/themes/agnoster.zsh-theme`を編集

     ```diff
     - vcs_info
     + function _precmd_vcs_info () { LANG=en_US.UTF-8 vcs_info }
     + add-zsh-hook precmd _precmd_vcs_info
     ```

#### `powerlevel10k`を導入する場合

1. レポジトリを clone

   ```console
   git clone https://github.com/romkatv/powerlevel10k.git ~/.oh-my-zsh/custom/themes/powerlevel10k
   ```

1. `~/.zshrc`からテーマを反映

   ```diff
   - ZSH_THEME="robbyrussell"
   + ZSH_THEME="powerlevel10k/powerlevel10k"
   ```

1. `source ~/.zshrc`すると, `powerlevel10k`の初期設定が始まります.

   - 再設定したい場合は`p10k configure`

### Vim

1. テーマ(ここでは[iceberg](https://github.com/cocopon/iceberg.vim))を適用

   ```console
   mkdir -p ~/.vim/colors
   git clone https://github.com/cocopon/iceberg.vim.git
   cp ./iceberg.vim/colors/iceberg.vim ~/.vim/colors
   echo "colorscheme iceberg" >> ~/.vimrc
   # rm -rf ./iceberg.vim
   ```

1. `neobundle`を導入

   ```console
   mkdir -p ~/.vim/bundle
   git clone https://github.com/Shougo/neobundle.vim ~/.vim/bundle/neobundle.vim
   ```

1. `~/.vimrc`をよしなに変更する

   参考にしたリンクを下に載せます.

   - <https://qiita.com/kotashiratsuka/items/dcd1f4231385dc9c78e7>
   - <https://qiita.com/snaka/items/c0c8760993f73780acbb>

### コンパイルの高速化

`#include <bits/stdc++.h>`をしてコンパイルをすると死ぬほど時間がかかるので, ヘッダをプリコンパイルしました. これでコンパイル時間が 50 秒から 6 秒になりました！！ (ちなみに`#include <iostream>`だけでも 10 秒かかってたのでかなり感動しています.)

```console
mkdir -p /root/.include/bits/stdc++.h.gch
cp `find / -name stdc++.h` /root/.include/bits/stdc++.h
cd /root/.include/bits
echo 'export CPATH=$CPATH:"/root/.include"' >> ~/.zshrc
# 実行しうるオプション「全て」に対して以下を行う
g++ <オプション> stdc++.h -o stdc++.h.gch/<何か好きな名前を付ける>.gch
```

### フォントを導入

[AnyFont](https://apps.apple.com/jp/app/anyfont/id821560738)というアプリを使いました. フォントによってはアプリ内で選択できないものもあるようなので注意です.

ちなみにぼくは`Meslo LG S NF Regular`というフォントを入れています.
