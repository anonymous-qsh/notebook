# VIM 使用技巧之IDEAVIM配置

Jetbrains 的很多产品还是非常好用的，IntelliJ IDEA, PyCharm, Clion 等等都非常受欢迎。 因为我比较喜欢 vim， 因此在使用这些 IDE 时都会装上 vim 的插件：ideavim. 不过因为我对 vim 的默认配置更改了很多，定制了很多快捷键等等，在使用默认配置下的 ideavim 时还是有些不太顺手，因此针对 ideavim 定制一些 vim 的配置便十分有必要了。

## `.ideavimrc` 配置文件

其实很简单，修改ideavim的配置文件`.ideavimrc`即可。默认情况下该文件并不存在，需要自行创建。macOS 或 Linux 下直接在当前用户目录下新建即可.

最近重新学习了一下vim的一些命令, 主要是在ideavim之下的一些配置, 发现vim的确有一些方便的操作, 在这里记录一下, 方便自己日后查看.

创建配置文件 `.ideavimrc` 后，接下来就是写入配置信息了。要注意，ideavim 只是 IDE 的插件，并没有实现原生 vim 的所有功能，有些 vim 的功能在 ideavim 中并不存在。比如 `<Leader>` 设置无效，需要在键位映射时指定按键。

## `surround.vim` 插件

仓库地址: <https://github.com/tpope/vim-surround>

这个插件可以快速添加,删除,修改成对字符, 配置方法如下:

```shell
set surround
```

使用说明:

`Surround.vim` is all about "surroundings": parentheses, brackets, quotes,
XML tags, and more.  The plugin provides mappings to easily delete,
change and add such surroundings in pairs.

It's easiest to explain with examples.  Press `cs"'` inside

    "Hello world!"

to change it to

    'Hello world!'

Now press `cs'<q>` to change it to

    <q>Hello world!</q>

To go full circle, press `cst"` to get

    "Hello world!"

To remove the delimiters entirely, press `ds"`.

    Hello world!

Now with the cursor on "Hello", press `ysiw]` (`iw` is a text object).

    [Hello] world!

Let's make that braces and add some space (use `}` instead of `{` for no
space): `cs]{`

    { Hello } world!

Now wrap the entire line in parentheses with `yssb` or `yss)`.

    ({ Hello } world!)

Revert to the original text: `ds{ds)`

    Hello world!

Emphasize hello: `ysiw<em>`

    <em>Hello</em> world!

Finally, let's try out visual mode. Press a capital V (for linewise
visual mode) followed by `S<p class="important">`.

    <p class="important">
      <em>Hello</em> world!
    </p>

This plugin is very powerful for HTML and XML editing, a niche which
currently seems underfilled in Vim land.  (As opposed to HTML/XML
*inserting*, for which many plugins are available).  Adding, changing,
and removing pairs of tags simultaneously is a breeze.

The `.` command will work with `ds`, `cs`, and `yss` if you install
[repeat.vim](https://github.com/tpope/vim-repeat).

## 配置文件

```vim
" 设置命令前导字符
let mapleader=' '

" 搜索高亮
set hlsearch
" 扩展搜索 支持更高级的正则表达式
set incsearch
" 忽略大小写
set ignorecase
" 智能大小写
set smartcase
set showmode
set number
" 相对行号
set relativenumber
" 到下面三行的时候才滚动
set scrolloff=3
" 保持历史命令
set history=100000
" 设置vim默认使用的缓冲区寄存器
set clipboard=unnamed
" 成对字符添加 修改 删除
set surround

" clear the highlighted search result 清除高亮搜索
nnoremap <Leader>sc :nohlsearch<CR>

" 保存
nnoremap <Leader>fs :w<CR>

" 跳转到方法
nnoremap <Leader>? :action GotoAction<CR>
" 调转到定义
nnoremap gd :action GotoDeclaration<CR>
" 调转到声明
nnoremap gi :action GotoImplementation<CR>

" project search
" 搜索全文
nnoremap <Leader>ps :action SearchEverywhere<CR>
" 跳转文件
nnoremap <Leader>pf :action GotoFile<CR>

" 查找引用
nnoremap <Leader>fu :action FindUsages<CR>

" Quit normal mode
" 退出 没有生效 (原因未知) 下文有快捷键冲突
nnoremap <Leader>q  :q<CR>
" 退出全部 没有生效 (原因未知)
nnoremap <Leader>Q  :qa!<CR>

" Move half page faster
" 向下跳转半屏
nnoremap <Leader>d  <C-d>
" 向上跳转半屏
nnoremap <Leader>u  <C-u>

" Insert mode shortcut
" 实现方向键
inoremap <C-h> <Left>
inoremap <C-j> <Down>
inoremap <C-k> <Up>
inoremap <C-l> <Right>
inoremap <C-a> <Home>
inoremap <C-e> <End>
inoremap <C-d> <Delete>

" Quit insert mode 退出插入模式
inoremap jj <Esc>
inoremap jk <Esc>
inoremap kk <Esc>

" Quit visual mode 退出view模式
vnoremap v <Esc>

" Move to the start of line 跳转到行首
nnoremap H ^

" Move to the end of line 跳转到行尾
nnoremap L $

" Redo 撤销
nnoremap U <C-r>

" Yank to the end of line 复制到行结束
nnoremap Y y$

" quit ==> close current window 这个感觉和其他的冲突
nnoremap <Leader>q <C-W>w

" Window operation 窗体操作
nnoremap <Leader>ww <C-W>w
nnoremap <Leader>wd <C-W>c
nnoremap <Leader>wj <C-W>j
nnoremap <Leader>wk <C-W>k
nnoremap <Leader>wh <C-W>h
nnoremap <Leader>wl <C-W>l
nnoremap <Leader>ws <C-W>s
nnoremap <Leader>w- <C-W>s
nnoremap <Leader>wv <C-W>v
nnoremap <Leader>w\| <C-W>v

" Tab operation tab 操作
nnoremap tn gt
nnoremap tp gT

" 下面都是IDE中快捷键的指令
" ==================================================
" Show all the provided actions via `:actionlist`
" ==================================================

" built in search looks better
nnoremap / :action Find<CR>
" but preserve ideavim search
nnoremap <Leader>/ /

nnoremap <Leader>;; :action CommentByLineComment<CR>

nnoremap <Leader>bb :action ToggleLineBreakpoint<CR>
nnoremap <Leader>br :action ViewBreakpoints<CR>

nnoremap <Leader>cv :action ChangeView<CR>

nnoremap <Leader>cd :action ChooseDebugConfiguration<CR>

nnoremap <Leader>ga :action GotoAction<CR>
nnoremap <Leader>gc :action GotoClass<CR>
nnoremap <Leader>gd :action GotoDeclaration<CR>
nnoremap <Leader>gf :action GotoFile<CR>
nnoremap <Leader>gi :action GotoImplementation<CR>
nnoremap <Leader>gs :action GotoSymbol<CR>
nnoremap <Leader>gt :action GotoTest<CR>

nnoremap <Leader>fp :action ShowFilePath<CR>

nnoremap <Leader>ic :action InspectCode<CR>

nnoremap <Leader>mv :action ActivateMavenProjectsToolWindow<CR>

nnoremap <Leader>oi :action OptimizeImports<CR>

nnoremap <Leader>pm :action ShowPopupMenu<CR>

nnoremap <Leader>rc :action ChooseRunConfiguration<CR>
nnoremap <Leader>re :action RenameElement<CR>
nnoremap <Leader>rf :action RenameFile<CR>

nnoremap <Leader>se :action SearchEverywhere<CR>
nnoremap <Leader>su :action ShowUsages<CR>
nnoremap <Leader>tc :action CloseActiveTab<CR>

nnoremap <Leader>tl Vy<CR>:action ActivateTerminalToolWindow<CR>
vnoremap <Leader>tl y<CR>:action ActivateTerminalToolWindow<CR>
```

存了一份其他大神整理的所有的Action: <https://gist.github.com/anonymous-qsh/a72c8aad7b61a145552581d57bf66877>[连接]
