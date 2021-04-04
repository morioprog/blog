---
title: "tmuxの備忘録"
date: 2020-06-18T02:12:00+09:00
categories: ["備忘録"]
---

## チートシート

* 標準の`prefix` : `Control + B`
* `prefix + ?`でヘルプ

### コマンド

| action | command |
| :-: | :- |
| セッション開始 | `tmux` |
| 名前を指定してセッション開始 | `tmux new-session -s <name>` |
| セッション一覧 | `tmux ls` |
| 接続クライアント一覧 | `tmux lsc` |
| セッションを再開 | `tmux a` |
| 指定したセッションを再開 | `tmux a -t <session-name>` |
| 指定したセッションを削除 | `tmux kill-session -t <session-name>` |
| 直近にアタッチしていたセッションを削除 | `tmux kill-session` |
| セッションを全て削除 | `tmux kill-server` |
| 設定ファイルを反映 | `tmux source ~/.tmux.conf` |

### セッション

| `prefix + <?>` | action |
| :-: | :-: |
| `s` | セッションの一覧選択 |
| `d` | セッションから離脱 |
| `$` | セッション名変更 |

### ウィンドウ

| `prefix + <?>` | action |
| :-: | :-: |
| `c` | ウィンドウ作成 |
| `w` | ウィンドウ一覧選択 |
| `&` | ウィンドウ強制終了 |
| `,` | ウィンドウ名変更 |
| `n` | 次のウィンドウに移動 |
| `p` | 前のウィンドウに移動 |
| `f` | ウィンドウ検索 |
| `<number>` | 指定した番号のウィンドウに移動 |

### ペイン

| `prefix + <?>` | action |
| :-: | :-: |
| `"` (`|`) | ペイン横分割 |
| `%` (`-`) | ペイン縦分割 |
| `z` | ペインの最大化(解除) |
| `<arrow>`(`hjkl`), `o` | ペインの移動 |
| `C-<arrow>`(`C-hjkl`) | ペインのリサイズ |
| `q` | ペイン番号を表示 |
| `q + <number>` | 指定した番号のペインに移動 |
| `x` | ペインを強制終了 |
| `t` | 時計を表示 |
| `{`, `}`, `C-o` | ペインの入れ替え |
| `Space` | レイアウトを変更 |

### コピー

| command | action |
| :-: | :-: |
| `prefix + [` | コピーモードに入る |
| `Space` (`v`) | 範囲選択モードに入る |
| `Enter` (`y`) | 選択範囲をコピーする |
| `prefix + ]` | ペースト |

---

## 導入

```sh
brew install tmux
```

## powerline

### フォント設定

```sh
git clone https://github.com/powerline/fonts.git
cd fonts
./install.sh
```

ターミナルの設定から使いたいフォントを選択.

### インストール

```sh
# pyenv global 3.8.2
pip install powerline-status
```

### カスタマイズ

```sh
cp -r ~/.pyenv/versions/3.8.2/lib/python3.8/site-packages/powerline/config_files ~/.config/powerline
ls ~/.config/powerline      #=> colors.json  colorschemes/  config.json  themes/
```

* 色 : `colorschemes/`
* 表示内容 : `themes/`

## `.tmux.conf`の設定

```sh
set-window-option -g mode-keys vi   # viモード
set -g prefix C-e                   # prefixキーをC-eに変更
unbind C-b                          # C-bのキーバインドを解除
bind | split-window -h              # ペイン横分割を`prefix + |`に
bind - split-window -v              # ペイン縦分割を`prefix + -`に

## 全ペイン同時入力のトグルを@にバインド
bind @ setw synchronize-panes \; display "synchronize-panes #{?pane_synchronized,on,off}"

## vimのキーバインドでペインを移動
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

## vimのキーバインドでペインをリサイズ
bind -r C-h resize-pane -L 5
bind -r C-j resize-pane -D 5
bind -r C-k resize-pane -U 5
bind -r C-l resize-pane -R 5

## コピーモード
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi y send-keys -X copy-selection-and-cancel

## マウス
set-option -g mouse on
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'copy-mode -e'"

## 256色表示
set -g default-terminal "screen-256color"
set-option -g terminal-overrides 'xterm*:colors=256'

## powerline
run-shell "powerline-daemon -q"
source ~/.pyenv/versions/3.8.2/lib/python3.8/site-packages/powerline/bindings/tmux/powerline.conf
```

---

## メモ

* 特殊な記号（セパレータなど）が表示できないとき
  * `tmux -u`でUTF-8を明示的に指定してセッションを開始する

## 参考

* <https://qiita.com/michiomochi@github/items/4bf8e34a91bbf3d9af20>
* <https://qiita.com/nmrmsys/items/03f97f5eabec18a3a18b>
* <https://qiita.com/Suzuki09/items/9bd5b04387582669d898>
* <https://qiita.com/waieneiaw/items/22ed18809739c9a69f25>
* <https://takuzoo3868.hatenablog.com/entry/2019/12/14/031652>
