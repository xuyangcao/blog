---
layout: post
---

## 前言

在众多Vim编辑器的自动补全插件中，[YouCompleteMe(YCM)](https://github.com/ycm-core/YouCompleteMe/)绝对是最好用的插件之一，但其配置过程往往令初学者望而却步。经过笔者多年折腾，至今基本达到满足日常使用水平。

在使用Vim+YCM时，YCM通常不能直接对第三方软件库（如在c++模式下的OpenCV）中的函数和定义进行自动联想和补全。为此，本文主要介绍如何使用ctags生成tags文件，并使其提升YCM在第三方软件库中的自动补全动能。

## 配置过程

本文以opencv为例，介绍如何通过生成tags文件来扩充YCM的自动补全功能。

### Tags文件的生成

#### 1 安装ctags

```bash
sudo apt-get install ctags
```

>注意，根据YCM的要求，需安装exuberant-ctags才可以$^{[1]}$。幸运的是，上述命令会默认安装exuberant-ctags。可以通过如下命令验证ctags的版本:
>```bash
>ctags --version
>```
>我这里输出如下：
>```bash
>Exuberant Ctags 5.9~svn20110310, Copyright (C) 1996-2009 Darren Hiebert
>Addresses: <dhiebert@users.sourceforge.net>, http://ctags.sourceforge.net
>Optional compiled features: +wildcards, +regex


#### 2 生成Tags文件

本文以opencv为例，首先下载opencv源码，然后在moudles路径下执行如下命令：

```bash
# 注意=+l中是小写的L，而不是1。
ctags -R --fields=+l *
```

> 注意，'--fields=+l'c++所需的参数，如果需要用c语言，还需要加上`--langmap=c:.c.h`参数

然后就会在当前文件夹下出现一个tags文件，我们观察一下Tags文件的部分内容，如下：

```tags
imread	imgcodecs/src/loadsave.cpp	/^Mat imread( const String& filename, int flags )$/;"	f	language:C++	namespace:cv
imread_	imgcodecs/src/loadsave.cpp	/^imread_( const String& filename, int flags, Mat& mat )$/;"	f	language:C++	namespace:cv
imreadmulti	imgcodecs/src/loadsave.cpp	/^bool imreadmulti(const String& filename, std::vector<Mat>& mats, int flags)$/;"	f	language:C++	namespace:cv
imreadmulti	imgcodecs/src/loadsave.cpp	/^bool imreadmulti(const String& filename, std::vector<Mat>& mats, int start, int count, int flags)$/;"	f	language:C++	namespace:cv
imreadmulti_	imgcodecs/src/loadsave.cpp	/^imreadmulti_(const String& filename, int flags, std::vector<Mat>& mats, int start, int count)$/;"	f	language:C++	namespace:cv
imshow	highgui/misc/java/src/java/highgui+HighGui.java	/^    public static void imshow(String winname, Mat img) {$/;"	m	language:Java	class:HighGui
imshow	highgui/src/window.cpp	/^void cv::imshow( const String& winname, InputArray _img )$/;"	f	language:C++	class:cv
imshow	highgui/src/window.cpp	/^void cv::imshow(const String& winname, const ogl::Texture2D& _tex)$/;"	f	language:C++	class:cv
imshow	videoio/src/cap_winrt_bridge.cpp	/^void VideoioBridge::imshow()$/;"	f	language:C++	class:VideoioBridge
imwrite	imgcodecs/src/loadsave.cpp	/^bool imwrite( const String& filename, InputArray _img,$/;"	f	language:C++	namespace:cv
imwrite_	imgcodecs/src/loadsave.cpp	/^static bool imwrite_( const String& filename, const std::vector<Mat>& img_vec,$/;"	f	language:C++	namespace:cv
imwritemulti	imgcodecs/include/opencv2/imgcodecs.hpp	/^bool imwritemulti(const String& filename, InputArrayOfArrays img,$/;"	f	language:C++	namespace:cv
```

可以看到，tags文件里包含了函数名称和基本定义，以及其在哪个文件中，YCM可以很好的解析它们。

#### 3. 配置tags文件路径

这一步，仅需要:

1. 将tags文件移动到自定义的一个位置

2. 在vimrc文件中指向它

例如，我将刚刚生成的tags文件改了个名字，并放在如下位置: `~/.vim/tagfiles/opencv.tags`,然后在vimrc中添加如下设置:

```vimrc
# 以下二选一即可
set tags=opencv.tags #相对位置，需要将tags文件放在当前工程下
set tags+=~/.vim/tagfiles/opencv.tags #绝对位置,tags文件就放在`~/.vim/tagfiles`内
```

### YCM配置

#### 配置

在vimrc下添加如下配置:

```vimrc
let g:ycm_collect_identifiers_from_tags_files = 1
```

这样，YCM会自动查找vimrc中定义的tags文件，并对其进行解析。可以在vim中运行如下命令查看YCM在当前缓冲区中加载了哪个tags文件：

```bash
:echo tagfiles()
```

我的电脑运行结果如下：

```bash
['/home/xuyangcao/.vim/tagfiles/opencv.tags']
```

#### 注意事项

根据YCM的文档，发现在编译过程中使用clangd completer比libclang completer要更智能一些，解析tags文件更全面。Clangd completer优势具体见参考资料[3]，这里不过多介绍。

因此，在编译YCM过程中，建议使用`--clanged-completer`参数。


## 参考资料

1 [YCM does not read identifiers from my tags files](https://github.com/ycm-core/YouCompleteMe/wiki/FAQ)

2 [Vim 配置关联多个tags文件](https://blog.csdn.net/suxw80then/article/details/41678329)

3 [C-family Semantic Completion](https://github.com/ycm-core/YouCompleteMe#c-family-semantic-completion)
