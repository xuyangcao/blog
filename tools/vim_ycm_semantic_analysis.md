---
layout: post
---

## 前言

**问题**: Vim编辑器在使用C++第三方库时（以OpenCV为例）发现YCM解析失效，找不到库文件。

**分析**: 通过YCM文档[1], 学习了YCM对代码进行语义分析的基本原理，如下：

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
set( CMAKE_EXPORT_COMPILE_COMMANDS ON
```

2. cmake一下，再编辑即可


### 方法二

Option 2: Provide the flags manually

这种方法我不用，读者可参考YCM文档，见资料[1]。


## 总结

本文通过分析YCM对C族语言进行语义分析的原理，解决了Vim编辑器使用C++第三方库时（以OpenCV为例）YCM解析失效，找不到库文件的情况。


## 参考资料

1. [YCM C-family Semantic Completion](https://github.com/ycm-core/YouCompleteMe#c-family-semantic-completion)
