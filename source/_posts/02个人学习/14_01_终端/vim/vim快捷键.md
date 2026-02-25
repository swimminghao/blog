---
title: vim快捷键
tags:
  - vim
categories: linux
abbrlink: e47927c
date: 2026-01-29 12:31:21
---

# vim快捷键

## 行跳转快捷键

- **文件行跳转**
    - **`gg`**: 跳转到文件第一行。
    - **`G`(Shift+g)** : 跳转到文件最后一行。
    - **`nG`(如`10G`)** : 跳转到第n行。
    - **`:n`(如`:10`+ 回车)** : 跳转到第𝑛行。
    - **`H`/ `M`/`L`** : 跳转到当前屏幕的最高行(High)、中间行(Middle)、最低行(Low)。
- **行内跳转**
    - **`0`(数字零)** : 跳转到当前行行首。
    - **`^`(Shift+6)** : 跳转到当前行第一个非空字符。
    - **`$`(Shift+4)** : 跳转到当前行行尾。
- **其他**
    - **`Ctrl+o`**: 返回到跳转前的位置。
    - **`%`**: 跳转到与之匹配的括号行

## vim-multiple-cursors插件快捷键

[vim-multiple-cursors](https://www.google.com/search?q=vim-multiple-cursors&rlz=1C5GCCA_en__1180__1180&oq=vim-multiple-cursors+使用&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIGCAEQABgeMgYIAhAAGB4yBggDEAAYHjIGCAQQABgeMgYIBRAAGB4yBggGEAAYHjIGCAcQABgeMgYICBAAGB4yBggJEAAYHtIBCDMxNjZqMGo3qAIAsAIA&sourceid=chrome&ie=UTF-8&mstk=AUtExfBRh8HwfHsJWr01OHADbRoRqNf_uozLaz0L911xUj33wwexDzlu_aiTTwjZttiXaGJpCJpDgUoVlyAIdmhnn69JaPuDwpce20ZnROos5VzTIc6LzwNxGrLoxw0RvTZiE-x7LkrTQUjuVISXcapLX5ojT5PG1m3KNINfCu6YTySy9JKvZqw753CCsbjAfARPmP2qSQvm06B4Yfc_7kcc7sczfGjGy4owWBbaMN862fHzSLcMmzw6pCoQx2yIo_1rsY3jc7edLcaCZpJcyX5VEuGR&csui=3&ved=2ahUKEwj9i52i-6-SAxWYIUQIHdhpApEQgK4QegQIARAB)插件在Vim 中实现了类似Sublime Text/VS Code 的多光标编辑功能。核心在于使用`Ctrl-n`选择下一个匹配项，`Ctrl-p`跳回上一个，`Ctrl-x`跳过当前项。进入多光标模式后，可使用`c`(修改)、`I`(行首插入)、`A`(行尾插入) 等命令批量编辑。

核心快捷键(Normal/Visual 模式)

- **`<C-n>`(Ctrl+n)** : 启动多光标模式并选择当前光标下的单词，或选择下一个匹配项。
- **`<C-p>`(Ctrl+p)** : 移除上一个光标，撤销上一次的[Ctrl-n](https://www.google.com/search?q=Ctrl-n&rlz=1C5GCCA_en__1180__1180&oq=vim-multiple-cursors+使用&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIGCAEQABgeMgYIAhAAGB4yBggDEAAYHjIGCAQQABgeMgYIBRAAGB4yBggGEAAYHjIGCAcQABgeMgYICBAAGB4yBggJEAAYHtIBCDMxNjZqMGo3qAIAsAIA&sourceid=chrome&ie=UTF-8&mstk=AUtExfBRh8HwfHsJWr01OHADbRoRqNf_uozLaz0L911xUj33wwexDzlu_aiTTwjZttiXaGJpCJpDgUoVlyAIdmhnn69JaPuDwpce20ZnROos5VzTIc6LzwNxGrLoxw0RvTZiE-x7LkrTQUjuVISXcapLX5ojT5PG1m3KNINfCu6YTySy9JKvZqw753CCsbjAfARPmP2qSQvm06B4Yfc_7kcc7sczfGjGy4owWBbaMN862fHzSLcMmzw6pCoQx2yIo_1rsY3jc7edLcaCZpJcyX5VEuGR&csui=3&ved=2ahUKEwj9i52i-6-SAxWYIUQIHdhpApEQgK4QegQIAxAC)操作。
- **`<C-x>`(Ctrl+x)** : 跳过当前匹配的单词。
- **`<C-g>`(Ctrl+g)** : 退出多光标模式，返回到单个光标。

使用流程

1. **移动光标**到想要修改的单词上。
2. 按下`<C-n>`选中该单词。
3. 继续按下`<C-n>`选中接下来的匹配项。
4. 按下`c`(修改), `i`(插入), `a`(追加) 等模式键进入编辑模式。
5. 输入新内容（所有光标处会同步修改）。
6. 按下`<Esc>`退出多光标编辑模式。

可视模式（Visual Mode）使用

在可视模式下选中一段文本，然后按`<C-n>`，该插件会在选中的每一行行尾添加光标，方便批量操作。

常见问题与技巧

- **退出**: 如果选错了，使用`<C-g>`取消。
- **兼容性**: 建议将默认的`<C-n>`映射为其他键，以防与插件快捷键冲突。
- **适用场景**: 批量修改变量名、在多行开头添加注释符、快速格式化数据。



## 在 Vim 中使用 NERDTree 时，切换焦点的常用方法有：

## 1. **标准切换方法**

- `Ctrl + w + w`：在所有窗口间循环切换
- `Ctrl + w + h/j/k/l`：向左/下/上/右切换窗口

## 2. **从 NERDTree 直接返回原文件**

- `t`：在新标签页打开文件（NERDTree 中）
- `T`：在后台标签页打开文件
- `i`：在水平分割窗口中打开
- `s`：在垂直分割窗口中打开

## 3. **快速切换回原窗口**

如果 NERDTree 在左侧，当前编辑文件在右侧：

- `Ctrl + w + l`：从 NERDTree 切换到右侧窗口

## 4. **NERDTree 特定命令**

- `q`：关闭 NERDTree 窗口
- 重新打开：`:NERDTreeToggle` 或 `:NERDTreeFocus`

## 5. **我的常用配置（在 `.vimrc` 中）**

vimrc

```
" 使用 Ctrl+n 切换 NERDTree
map <C-n> :NERDTreeToggle<CR>
" 自动切换焦点到文件
let NERDTreeQuitOnOpen = 1  " 打开文件后自动关闭 NERDTree
```



**最简单的方式**：直接按 `Ctrl + w + w` 即可在 NERDTree 和编辑窗口间切换。

