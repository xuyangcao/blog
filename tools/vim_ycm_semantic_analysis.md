---
layout: post
---

## 前言


在众多Vim编辑器的自动补全插件中，[YouCompleteMe(YCM)](https://github.com/ycm-core/YouCompleteMe/)绝对是最好用的插件之一，但其配置过程往往令初学者望而却步。经过笔者多年折腾，至今基本达到满足日常使用水平。

如果读者对YouCompleteMe这个插件不熟悉，建议首先参考笔者的博客[Vim: 配置Python和C++常用插件](https://xuyangcao.github.io/blog/tools/vim_configs/)。

在使用Vim编辑C族语言代码时，如果额外使用了某些第三方库（如OpenCV），会发现YCM解析失效，找不到库文件，后序自动联想也无法使用。为此，本文结合YCM文档，首先分析一下为什么YCM在此情况下会失效，然后再给出对应的解决方案。


## YCM语义分析原理

通过YCM文档[1], 学习了YCM对代码进行语义分析的基本原理，如下：

- YCM使用`clangd`工具（依赖clang编译器）对代码进行语义分析，如代码补全，`GoTo`，代码诊断等。如果`clangd`无法解析代码，则YCM就无法提供语义分析功能。

- 而clang编译器需要一些列compile flags才能对代码进行解析，我们遇到问题的原因正是编译器没有找到相应的compile flags，进而出现解析失效，无法找到库文件的现象。


## 方法

这里我直接摘抄YCM文档，如下：

### 方法一

Option 1: Use a [compilation database](https://clang.llvm.org/docs/JSONCompilationDatabase.html)

- If using CMake, add -DCMAKE_EXPORT_COMPILE_COMMANDS=ON when configuring (or add set( CMAKE_EXPORT_COMPILE_COMMANDS ON ) to CMakeLists.txt) and copy or symlink the generated database to the root of your project.

- If using Ninja, check out the compdb tool (-t compdb) in its docs.

- If using GNU make, check out compiledb or Bear.

- For other build systems, check out .ycm_extra_conf.py below.


根据上述内容，我常使用CMake方式，操作如下：

1. 在配置好的CMakeLists.txt中添加如下内容：

```bash
set( CMAKE_EXPORT_COMPILE_COMMANDS ON )
```

2. cmake一下，再编辑即可


### 方法二

Option 2: Provide the flags manually

这种方法我不用，读者可参考YCM文档，见资料[1]。


## 总结

本文通过分析YCM对C族语言进行语义分析的原理，解决了Vim编辑器使用C++第三方库时（以OpenCV为例）YCM解析失效，找不到库文件的情况。


## 参考资料

1. [YCM C-family Semantic Completion](https://github.com/ycm-core/YouCompleteMe#c-family-semantic-completion)
