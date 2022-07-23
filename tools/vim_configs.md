---
layout: post
---

## 前言
本文中所有的插件均为作者长时间实践挑选的产物，适合入门Vim编辑器之后的人群。通过增加不同插件，几乎可以完全抛弃鼠标，使代码效率倍增。 
以下所有配置均在ubuntu16.04下安装完成。

## 配置vim插件管理工具

常用的vim插件管理工具有两个，一个是[vim-pathogen](https://github.com/tpope/vim-pathogen)另一个是[Vundle.vim](https://github.com/VundleVim/Vundle.vim).

### 配置vim-pathogen

对比了一下两个工具，这里我选择的是[vim-pathogen](https://github.com/tpope/vim-pathogen),其配置过程简单描述如下，详细信息直接参考官网就可以了：

* 首先在`~/.vim`下创建两个文件夹，一个是autoload，另一个是bundle，以后所有的插件应用都安装在bundle中。然后下载pathogen到autoload下面。

   ```bash
    $ mkdir -p ~/.vim/autoload ~/.vim/bundle && \
    $ curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
    ```

* 配置`.vimrc`,在`.vimrc`添加如下代码：

    ```bash
    $ execute pathogen#infect()
    $ syntax on
    $ filetype plugin indent on
    ```

* 基本配置完上述内容就可以使用了，详情请参考[文档](https://github.com/tpope/vim-pathogen)。

### 配置vimogen

> 可选，后期我基本不用这个了

[vimogen](https://github.com/rkulla/vimogen)结合上面的pathogen使得安装插件更加容易，只要在一个固定的文件`.vimogen_repos`中添加上插件的github地址，然后就可以使用命令自动安装。在安装好pathogen之后，添加如下代码即可：

* 安装

   ```bash
    $ git clone https://github.com/rkulla/vimogen
    $ chmod u+x vimogen
    $ cp vimogen to your $PATH
    ```

* 使用

    首先将想要安装软件的地址放在`~/.vimogen_repos`里面（见[我的github](https://github.com/xuyangcao/vimrc)），像下面这样：

    ```
    https://github.com/Valloric/YouCompleteMe
    https://github.com/jeaye/color_coded
    https://github.com/rdnetto/YCM-Generator
    https://github.com/scrooloose/nerdtree
    https://github.com/Shougo/neocomplete.vim
    ```

    然后执行`vimogen`：

    ```
    $ vimogen
    ```
    安装界面如下：
    ```
    1) INSTALL
    2) UNINSTALL
    3) UPDATE
    4) EXIT
    Enter the number of the menu option to perform:
    ```
    直接按`1`就可以了，程序会自动安装上面链接里面的插件，如果已经安装，则会跳过。
    如果按`2`，则是卸载已经安装的插件，直接选择要卸载的插件对应的编号即可。


	> 注: 在一些环境下，若不方便联网，可以放弃使用vimogen工具，直接在`~/.vim/bundle`路径下通过`git clone`将插件下载到本地即可。需要特别注意的是，如果在windows系统下载插件再上传到Linux系统，可能因为结束符不同而出现不兼容的现象。 


## 配置常用插件

### 1. YouCompleteMe           

[YouCompleteMe](https://github.com/Valloric/YouCompleteMe)基本上支持各种语言的自动补全，其安装过程需要编译一下，因此这里介绍一下安装过程，后面的几个插件因为直接使用vimogen装上就可以直接使用，因此不在过多介绍后面几类插件。`YCM`安装过程如下：

* 下载

    直接使用[vimogen一节](##配置vimogen)介绍的方法讲`YCM`下载到`~/.vim/bundle`中，然后进入到`~/.vim/bundle/YouCompleteMe/`路径下。

* 安装 

    按照自己的需求选择编译命令，这里我因为常用的是python，但是也把c-family语言也选上了，因此执行下面命令进行编译安装：

    ```bash
    cd ~/.vim/bundle/YouCompleteMe
    python3 install.py --clang-completer
    # 注意：如果编译开始时出错一般是缺少各种工具，按照提示安装就可以了。
    ```

* 配置

    这里根据自己的需求配置一些常用参数在`.vimrc`中。
    我配置的如下：

    ```vim
    "==================YCM====================
    let g:ycm_global_ycm_extra_conf = '~/.vim/bundle/YouCompleteMe/.ycm_extra_conf.py'
    nnoremap <F12> :YcmCompleter GoToDefinitionElseDeclaration<CR>
    ```

    配置的第二行配置了一个快捷键用来使用`F12`实现go to definition的功能。

    自动补全的效果如下：

    <div class="fig figcenter">
        <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC1lMjZlZDY0NDQyZmFlYTgyLnBuZw?x-oss-process=image/format,png" width=600px>
    </div>
    <div class="fig figcenter">
        <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC04YmQwZDIzNGQ4MmUwMThmLnBuZw?x-oss-process=image/format,png" width=600px>
        <div class="figcaption">YCM自动补全</div>
    </div>


**常见问题：**

1. 在有一次配置服务器上的vim编辑器时候，遇到下面的问题：

	> YouCompleteMe unavailable no module named builtins.

	查资料发现：这是因为网络很慢，导致要依赖的文件还没有下载全，所以可以先把文件下载了。

	**解决办法**：
	
	```
	$ cd /home/yourusername/.vim/bundle/YouCompleteMe
	$ git submodule update --init --recursive
	```

2. 无法下载`libclang-12.0.0-x86_64-unknown-linux-gnu.tar.bz2`，遇到的问题大概如下：

	> Cannot find path to libclang in prebuilt binaries

	**解决方法**：手动下载所需文件放在报错的路径下即可，这里我找到的下载链接如下：
	*链接：*
	
	> https://github.com/ycm-core/llvm/releases/tag/12.0.0
	
	*路径*
	
	> ~/.vim/bundle/YouCompleteMe/third_party/ycmd/clang_archives


### 2. neocomplete.vim

   * [neocomplete.vim](https://github.com/Shougo/neocomplete.vim)实现路径的自动补全功能，效果如下：
    
<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC00OTRmMzRhZWE5YjUwNzgwLnBuZw?x-oss-process=image/format,png" width=600px>
    <div class="figcaption">neocomplete</div>
</div>


### 3. nerdtree

* [nerdtree](https://github.com/scrooloose/nerdtree)实现在左侧目录树功能，其效果如下：

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC02OGQzYTQyMWMyYTZiNjQ0LnBuZw?x-oss-process=image/format,png" width=600px>
    <div class="figcaption">nerdtree</div>
</div>


* 使用方法：

  1. 按`m`显示出菜单，然后根据菜单提示可以进行更多操作，比如这里我想新建一个文件(夹)，直接按`a`, 然后输入想新建的文件(夹)即可。

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC0xMjM1ZTA4Njk4OGFiZTBkLnBuZw?x-oss-process=image/format,png" width=600px>
    <div class="figcaption">nerdtree</div>
</div>


### 4. vim-airline

[vim-airline](https://github.com/vim-airline/vim-airline)
主要负责状态栏显示

### 5. vim-gitgutter

[vim-gitgutter](https://github.com/airblade/vim-gitgutter)
主要负责git状态显示

### 6. vim-signify

[vim-signify](https://github.com/mhinz/vim-signify)
和vim-gitgutter结合使用

### 7. vim-fugitive

[vim-fugitive](https://github.com/tpope/vim-fugitive)
更强大的git管理

### 8. auto-pairs

[auto-pairs](https://github.com/jiangmiao/auto-pairs)
括号，引号等自动补全，很实用

### 9. indentLine

[indentLine](https://github.com/Yggdroot/indentLine)
显示缩进

### 10. vim-colorschemes

[vim-colorschemes](https://github.com/flazz/vim-colorschemes)
代码配色方案

### 11. taglist.vim 

[taglist.vim](https://github.com/vim-scripts/taglist.vim.git)
Tag信息

### 12. vim-commentary

[vim-commentary](ttps://github.com/tpope/vim-commentary.git)
注释

### 13. a.vim

[a.vim](https://github.com/vim-scripts/a.vim.git)

在.c和.h之间跳转

### 14 markdown-preview.nvim

[markdown-preview.nvim](https://github.com/iamcco/markdown-preview.nvim.git)
markdown 预览

>  **注意事项** 这个插件安装时遇到个小插曲，安装完无法使用，具体解决方法见issue，链接如下：[https://github.com/iamcco/markdown-preview.nvim/issues/45](https://github.com/iamcco/markdown-preview.nvim/issues/45)
>  解决方案：执行`~/.vim/bundle/markdown-preview.nvim/app`下面的`install.sh`安装所需插件即可。


### 最终效果

* 通过配置上面插件，我经常使用的界面大概长成下面的样子。

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC04YzMxYjFhNjQyNDExYmQyLnBuZw?x-oss-process=image/format,png" width=600px>
    <div class="figcaption">vim界面</div>
</div>



## .vimrc

最后把常用的`.vimrc`文件配置贴出来用于给读者参考. 感兴趣的读者可以去[我的github](https://github.com/xuyangcao/vimrc)上下载。

```vim
"============= pathogen ============
execute pathogen#infect()
syntax on
filetype plugin indent on

"=========== basic config ==========
set encoding=utf-8      " encoding
set number              " show line number
set ruler               " Show the line and column number of the cursor position
set shiftwidth=4        " Number of spaces to use for each step of (auto)indent
set softtabstop=4       " Number of spaces that a <Tab> counts for whild editing
set tabstop=4           " Number of spaces that a <Tab> in the file counts for
set expandtab           " In Insert mode: Use the appropriate number of spaces to insert a <Tab>
set hlsearch            " Last search pattern highlighting
set showmatch           " When a bracket is inserted, briefly jump to the matching one
set foldmethod=indent   " The kind of folding used for the current window
set foldlevel=99        " Sets the fold level
set nofen               " All folds are open
set history=50	        " Keep 50 lines of command line history
set showcmd             " Display incomplete commands
set incsearch	        " Do incremental searching
set noswapfile          " Open a new buffer without creating a swap file for it
set modeline            " set modeline
set nocompatible        " Use Vim settings, rather then Vi settings
set wildmenu            " Command-line completion operates in an enhanced mode
set ttimeout            " Timeout on key codes
set ttimeoutlen=100     " The time in milliseconds that is waited for a key code or mapped key sequence to complete
set autowrite           " Write the contents of the file, if it has been modified
syntax enable           " Enable syntax

" Highlight current line
autocmd InsertEnter * se cul            
" Comments according to different lauguages
autocmd FileType cpp set commentstring=//\ %s
autocmd FileType php set commentstring=//\ %s
autocmd FileType python set commentstring=#\ %s

" When the document is reopened, the cursor returns to the position before the document was closed
if has("autocmd")
 autocmd BufReadPost *
 \ if line("'\"") > 0 && line ("'\"") <= line("$") |
 \ exe "normal g'\"" |
\ endif
endif

" Copy & paste using the true clipboard, vim-gtk required
map <Leader>y "+y
map <Leader>p "+p

" Compile shortcuts
autocmd filetype python nnoremap <F5> :w <bar> exec '!python '.shellescape('%')<CR>
autocmd filetype cpp nnoremap <F5> :w <bar> exec '!g++ --std=c++11 -pthread '.shellescape('%').' -o ./bin/'.shellescape('%:r').' && ./bin/'.shellescape('%:r')<CR>
autocmd filetype cc nnoremap <F5> :w <bar> exec '!g++ --std=c++11 -pthread '.shellescape('%').' -o ./bin/'.shellescape('%:r').' && ./bin/'.shellescape('%:r')<CR>
autocmd filetype dot nnoremap <F5> :w <bar> exec '!dot -Tsvg sqlparse.dot > sqlparse.svg'<CR>
autocmd Filetype java nnoremap <F5> :w <bar> exec '!javac '.shellescape('%'). ' -d ./bin'<CR>
autocmd filetype java nnoremap <F2> :w <bar> exec '!java -cp ./bin '.shellescape('%:r')<CR>

"=============== YCM ================
let g:ycm_global_ycm_extra_conf = '~/.vim/bundle/YouCompleteMe/.ycm_extra_conf.py'
nnoremap <F12> :YcmCompleter GoToDefinitionElseDeclaration<CR>
nmap <leader>yfw <Plug>(YCMFindSymbolInWorkspace)
nmap <leader>yfd <Plug>(YCMFindSymbolInDocument)
let g:ycm_collect_identifiers_from_tags_files = 1

"============== nerdtree ============
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 1 && isdirectory(argv()[0]) && !exists("s:std_in") | exe 'NERDTree' argv()[0] | wincmd p | ene | endif
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
map <C-n> :NERDTreeToggle<CR> 
let NERDTreeWinPos="left"
let NERDTreeIgnore=['\.vim$', '\~$', '\.o$', '\.d$', '\.a$', '\.out$', '\.tgz$']

"====== ultisnips & vim-snippets =====
"ultisnips
let g:UltiSnipsSnippetDirectories=[$HOME.'/.vim/ultisnips']
let g:UltiSnipsExpandTrigger="<c-o>"
let g:UltiSnipsJumpForwardTrigger="<c-j>"
let g:UltiSnipsJumpBackwardTrigger="<c-k>"
let g:UltiSnipsEditSplit="vertical" 
nmap <c-e> :UltiSnipsEdit<CR>
"vim-snippets
let g:snips_author = "xuyangcao"
let g:snips_email = "caoxuyang@bjtu.edu.cn"
let g:snips_github = "https://xuyangcao.github.io"

"================ tags ===============
set tags=tags
set tags+=~/.vim/tagfiles/opencv.tags

"============== taglist ==============
nnoremap  <leader>t  :TlistToggle <CR> 
let Tlist_Inc_Winwidth=0
let Tlist_Use_Right_Window=1
let Tlist_File_Fold_Auto_Close=1
let g:tlist_markdown_settings = 'markdown;h:Headlins'

"============ colorscheme ============
colorscheme molokai

"========== airlinae theme =========== 
let g:airline_theme='molokai'

"============= Markdown ============== 
"Markdown Preview
nmap <F5> :MarkdownPreview<CR>
"vim markdown
let g:vim_markdown_toc_autofit = 1
let g:vim_markdown_conceal = 0
let g:vim_markdown_new_list_item_indent = 0
"vim markdown toc
nmap toc :GenTocMarked<CR>
```

```code
# .vimogen_repos

https://github.com/Valloric/YouCompleteMe
https://github.com/SirVer/ultisnips.git
https://github.com/scrooloose/nerdtree
https://github.com/Shougo/neocomplete.vim

https://github.com/airblade/vim-gitgutter
https://github.com/tpope/vim-fugitive

https://github.com/Yggdroot/indentLine
https://github.com/tpope/vim-commentary.git
https://github.com/jiangmiao/auto-pairs

https://github.com/vim-scripts/taglist.vim.git
https://github.com/vim-scripts/a.vim.git

https://github.com/vim-airline/vim-airline
https://github.com/vim-airline/vim-airline-themes
https://github.com/flazz/vim-colorschemes  

https://github.com/iamcco/markdown-preview.nvim.git
```


## 参考资料

- [Building-Vim-from-source](https://github.com/Valloric/YouCompleteMe/wiki/Building-Vim-from-source)
- [[ubuntu]自己编译安装vim 8.0的方法](https://blog.csdn.net/a464057216/article/details/52821171)
- [github: vim-pathogen](https://github.com/tpope/vim-pathogen)
- [github: Vundle.vim](https://github.com/VundleVim/Vundle.vim)
- [YouCompleteMe A code-completion engine for Vim](http://valloric.github.io/YouCompleteMe/)
- [跟我一起学习VIM - vim插件合集](https://blog.csdn.net/mergerly/article/details/51671890)
- [vim查找/替换字符串](https://www.cnblogs.com/GODYCA/archive/2013/02/22/2922840.html)
- [Vim技巧之分割窗口](http://blog.chinaunix.net/uid-24673811-id-1994607.html)
- [vim 多窗口编辑](https://blog.csdn.net/shuangde800/article/details/11430659)
- [VIM选择文本块/复制/粘贴](https://blog.csdn.net/lcj_cjfykx/article/details/9091569)
- [ultisnips](https://github.com/SirVer/ultisnips)
- [vimtex](https://github.com/lervag/vimtex)
- [Auto-call auto-completion v2](https://asciinema.org/a/202405)
- [anyone_else_abusing_ultisnips?](https://www.reddit.com/r/vim/comments/8zs2fa/anyone_else_abusing_ultisnips/)
- [vim-airline](https://github.com/vim-airline/vim-airline)
- [dotfiles](https://github.com/windelicato/dotfiles)
- [vim-latex-live-preview](https://github.com/xuhdev/vim-latex-live-preview)
- [fugitive-vim-working-with-the-git-index](http://vimcasts.org/episodes/fugitive-vim-working-with-the-git-index/)
- [vimcasts.org](http://vimcasts.org/)
- [vim的powerline插件没有颜色](https://blog.csdn.net/MENGHUANBEIKE/article/details/53023855)
- [Creating a new file or directory in Vim using NERDTree](https://sookocheff.com/post/vim/creating-a-new-file-or-directoryin-vim-using-nerdtree/)
- [Using Spell Checking in Vim](https://www.linux.com/learn/using-spell-checking-vim)
- [Vim 实用插件推荐（2017）](https://zhuanlan.zhihu.com/p/24742679)
- [Tmux使用手册](https://louiszhai.github.io/2017/09/30/tmux/)
- [YouCompleteMe（YCM）安装时遇到的问题（能遇到的几乎都遇到了）](https://blog.csdn.net/nbu_dahe/article/details/119615423)
- [https://github.com/iamcco/markdown-preview.nvim/issues/171
](https://github.com/iamcco/markdown-preview.nvim/issues/171
)
