---
title: "Vim"
date: 2014-07-14 20:34:42
---

## 快捷键

### 移动

* `0`  - Move to *beginging* of line, 也可以使用 `Home`.
* `^`  - 在有tab或space的代码行里, `0`是移到最行首, 而`^`是移到代码行首
* `$`  - Move to *end* of line
* `gg` - Move to *first* line of file
* `G`  - Move to *last* line of file
* `ngg`- 移动到指定的第n行, 也可以用`nG`
* `w`  - Move *forward* to next word
* `b`  - Move *backward* to next word
* `%`  - 在匹配的括号、块的首尾移动
* `C-o`- 返回到上次移动前的位置, 也可以用两个单引号`'`
* `C-i`- 前进到后一次移动的位置

### 搜索

* `*`     - Search *forward* for word under cursor
* `#`     - Search *backward* for word under curor
* `/word` - Search *forward* for *word*. Support *RE*
* `?word` - Search *backward* for *word*. Support *RE*
* `n`     - Repeat the last `/` or `?` command  
* `N`     - Repeat the last `/` or `?` command in opposite direction

在搜索后, 被搜索的单词都会高亮, 一般想取消那些高亮的单词, 可以再次搜索随便输入一些字母, 搜索不到自然就取消了. 另外也可以使用 `nohl` 取消这些被高亮的词.

### 删除

* `x`  - Delete character *forward*(under cursor), and remain in normal mode
* `X`  - Delete character *backward*(before cursor), and remain in normal mode
* `r`  - Replace single character under cursor, and remain in normal mode
* `s`  - Delete single character under cursor, and *switch* to insert mode
* `dw` - Delete a *word* forward
* `daw`- 上面的`dw`是删除一个单词的前向部分, 而这个是删除整个单词, 不论cursor是否在单词中间
* `db` - Delete a *word* backward
* `dd` - Delete *entire* current line
* `D`  - Delete until end of line


### 复制粘贴

* `y`   - Yank(copy)
* `yy`  - Yank current line
* `nyy` - Yank `n` lines from current line
* `p`   - Put(paste) yanked text *below* current line
* `P`   - Put(paste) yanked text *above* current line
* `"+p` - past coped text from system clipboard
* `"+Y"`- Copy current line to system clipboard


### 更多资料

* [What is your most productive shortcut with Vim?](http://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim)

## 插件

* Vundle 插件自动管理工具
* vim-powerline 状态栏增强
* tagbar tags生成与浏览
* gundo undo管理
* nerdtree 文件目录导航
* YouCompleteMe 自动补全
