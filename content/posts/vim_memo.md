---
title: "Vimの備忘録"
date: 2020-06-18T02:13:00+09:00
thumbnail: "img/header/vim.svg"
categories: ["備忘録"]
---

## チートシート

- `10<command>`で`<command>`を 10 回実行

### カーソル移動

#### 基本移動

| command |            action            |
| :-----: | :--------------------------: |
|   `h`   |           左に移動           |
|   `j`   |           下に移動           |
|   `k`   |           上に移動           |
|   `l`   |           右に移動           |
|  `gj`   | (表示上の行について)下に移動 |
|  `gk`   | (表示上の行について)上に移動 |

#### 行移動

| command |               action                |
| :-----: | :---------------------------------: |
|   `0`   |     行頭に移動 (インデント無視)     |
|   `^`   |             行頭に移動              |
|   `$`   |             行末に移動              |
|   `+`   | 下の行頭に移動 (行番号が増える方向) |
|   `-`   |  上の行頭に移動 (行番号が減る方向)  |

#### 単語移動

大文字(`W`, `B`, `E`, `gE`)にすると, 単語の定義が変わる.

| command |        action        |
| :-----: | :------------------: |
|   `w`   |   単語の先頭に進む   |
|   `b`   |   単語の先頭に戻る   |
|   `e`   |   単語の末尾に進む   |
|  `ge`   |   単語の末尾に戻る   |
|   `%`   | 対応するカッコに移動 |

#### まとまった移動

|   command   |                  action                  |
| :---------: | :--------------------------------------: |
|    `gg`     |           ファイルの先頭に移動           |
|     `G`     |           ファイルの末尾に移動           |
|    `zz`     | 現在の行が画面中央になるようにスクロール |
| `:<number>` |             指定した行に移動             |
|    `C-u`    |         半画面分上に移動 (`up`)          |
|    `C-d`    |        半画面分下に移動 (`down`)         |
|    `C-b`    |        一画面分上に移動 (`back`)         |
|    `C-f`    |        一画面分下に移動 (`front`)        |
|     `{`     |            段落ごとに上に移動            |
|     `}`     |            段落ごとに下に移動            |

#### 検索移動

##### 行ごと

|   command    |                  action                  |
| :----------: | :--------------------------------------: |
| `f + <char>` |      カーソルから右の`<char>`に移動      |
| `F + <char>` |      カーソルから左の`<char>`に移動      |
| `t + <char>` | カーソルの一文字前から右の`<char>`に移動 |
| `T + <char>` | カーソルの一文字前から左の`<char>`に移動 |
|     `;`      |           順方向に繰り返し検索           |
|     `,`      |           逆方向に繰り返し検索           |

##### ファイル全体

|   command   |              action               |
| :---------: | :-------------------------------: |
| `/ + <str>` | カーソルから順方向に`<str>`を検索 |
| `? + <str>` | カーソルから逆方向に`<str>`を検索 |
|     `*`     |      カーソル下の単語を検索       |
|     `n`     |       順方向に繰り返し検索        |
|     `N`     |       逆方向に繰り返し検索        |

### 編集

|  command  |                action                 |
| :-------: | :-----------------------------------: |
|    `J`    |               行を連結                |
|    `D`    |       カーソルから行末まで削除        |
| `yy`, `Y` |               行コピー                |
| `dd`, `D` |              行切り取り               |
|    `x`    |        カーソル下の文字を削除         |
|    `p`    |               ペースト                |
|   `>>`    |         インデントを深くする          |
|   `<<`    |         インデントを浅くする          |
|    `.`    | 直前の変更を繰り返す (`x`, `2dd`など) |
|    `u`    |                 undo                  |
|   `C-r`   |                 redo                  |

### 領域選択

| command |          action          |
| :-----: | :----------------------: |
|   `v`   |       領域選択開始       |
|   `V`   |        行選択開始        |
|  `C-v`  |       矩形選択開始       |
|   `y`   |          コピー          |
|   `d`   |         切り取り         |
|   `=`   | 選択範囲を自動インデント |

### ウィンドウ

|    command     |            action            |
| :------------: | :--------------------------: |
|    `:split`    |       画面を上下に分割       |
|   `:vsplit`    |       画面を左右に分割       |
|    `:close`    |      ウィンドウを閉じる      |
| `:new <file>`  | 新規ウィンドウ作成(垂直方向) |
| `:vnew <file>` | 新規ウィンドウ作成(水平方向) |
|  `:e <file>`   |        ファイルを開く        |
|    `:hide`     |       ウィンドウを隠す       |
|    `C-w +`     |       ウィンドウを拡大       |
|    `C-w -`     |       ウィンドウを縮小       |
|   `C-w hjkl`   |       ウィンドウを移動       |
|    `C-w r`     |     ウィンドウを入れ替え     |

### レジスタ

|  command   |          action          |
| :--------: | :----------------------: |
| `"<reg>y`  | 選択範囲をレジスタに保存 |
| `"<reg>yy` | 現在の行をレジスタに保存 |
| `"<reg>p`  | レジスタの内容をペースト |
|   `:reg`   | レジスタの情報を一覧表示 |

### その他

|       command       |                    action                     |
| :-----------------: | :-------------------------------------------: |
| `:%s/<from>/<to>/g` |                  単語の置換                   |
|        `zf`         |             選択範囲の折りたたみ              |
|       `Space`       |               折りたたみの展開                |
|  `q<reg><macro>q`   |     操作の記録(`<macro>`)をレジスタに保存     |
|      `@<reg>`       |        レジスタに保存された操作を実行         |
|    `:!<command>`    |              外部コマンドを実行               |
|   `:r!<command>`    | 外部コマンドを実行 (出力をカーソル位置に挿入) |

---

## Vundle の導入

```sh
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

## `~/.vimrc`の設定

```vim
"" Be iMproved
set nocompatible

"" Disable filetype
filetype off
filetype plugin indent off

"" Use utf-8
set encoding=utf-8      " when loading
scriptencoding=utf-8    " when coding
set fileencoding=utf-8  " when saving

"" Set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

"" Plugins (add line and :PluginInstall)
""" Basic
Plugin 'VundleVim/Vundle.vim'
Plugin 'thinca/vim-quickrun'                " :QuickRun
" Plugin 'zxqfl/tabnine-vim'                  " smart completion
Plugin 'bronson/vim-trailing-whitespace'    " highlight traling whitespace (:FixWhitespace to delete)
" Plugin 'Yggdroot/indentLine'                " visualize indent
""" Language
Plugin 'othree/yajs.vim'                    " JavaScript
Plugin 'octol/vim-cpp-enhanced-highlight'   " C++
Plugin 'rust-lang/rust.vim'                 " Rust
Plugin 'petdance/vim-perl'                  " Perl
Plugin 'Raku/vim-raku'                      " Raku

"" All plugins must be added before the following line
call vundle#end()

"" Basic config
set number              " show row number
set relativenumber      " show relative row number
set title               " show file name
colorscheme iceberg     " set colorscheme
syntax on               " enable syntax highlight
set cursorline          " highlight current row

"" Tab = space x 4
set expandtab           " C-v + <Tab> to enter tab
set tabstop=4           " tab         = space x 4
set shiftwidth=4        " indent      = space x 4
set softtabstop=4       " cursor move = space x 4
set autoindent          " same indent as previous row
set smartindent         " `{` -> indent++, `}` -> indent--

"" String search
set incsearch           " incremental search
set ignorecase          " ignore case
set smartcase           " case-sensitive when pattern includes capital
set hlsearch            " highlight result
" <ESC><ESC> : toggle highlight
nnoremap <silent><Esc><Esc> :<C-u>set nohlsearch!<CR>

"" Cursor
set whichwrap=b,s,h,l,<,>,[,],~     " move right at end of line to move to next line
" move by display lines
nnoremap j gj
nnoremap k gk
nnoremap <down> gj
nnoremap <up> gk

"" Parentheses
set showmatch                           " show corresponding parenthesis when typing closing parenthesis
source $VIMRUNTIME/macros/matchit.vim   " extend `%` (def <=> end)

"" Command completion
set wildmenu            " command completion
set history=5000        " size of command history

"" Disable autoindent when paste (from clipboard)
if &term =~ "xterm"
    let &t_SI .= "¥e[?2004h"
    let &t_EI .= "¥e[?2004l"
    let &pastetoggle = "¥e[201~"

    function XTermPasteBegin(ret)
        set paste
        return a:ret
    endfunction

    inoremap <special> <expr> <Esc>[200~ XTermPasteBegin("")
endif

"" Run zsh aliased commands from vim command mode
"" https://vi.stackexchange.com/questions/16186/how-to-run-zsh-aliased-command-from-vim-command-mode
autocmd vimenter * let &shell='/bin/zsh'

"" Enable mouse
set mouse+=a
set ttymouse=xterm2

"" Enable filetype
filetype plugin indent on
```

---

## 参考

- <https://qiita.com/ahiruman5/items/4f3c845500c172a02935>
- <https://qiita.com/takeharu/items/9d1c3577f8868f7b07b5>
- <http://vimblog.hatenablog.com/entry/vimrc_set_tab_indent_options>
- <https://vi.stackexchange.com/questions/16186/how-to-run-zsh-aliased-command-from-vim-command-mode>
- <http://cohama.hateblo.jp/entry/20130108/1357664352>
- [https://kaworu.jpn.org/vim/vim%E3%81%A7%E3%83%9E%E3%82%A6%E3%82%B9%E3%82%92%E6%9C%89%E5%8A%B9%E3%81%AB%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95](https://kaworu.jpn.org/vim/vimでマウスを有効にする方法)
