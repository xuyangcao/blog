---
layout: post
---

## 前言

Git原理用一张图就可以解释明白了：

<div class="fig figcenter">
    <img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xNDI1MDM1OC1jODk5OTI2MjgwY2FmN2QxLmpwZWc?x-oss-process=image/format,png">
    <div class="figcaption">description</div>
</div>

## 常用命令

下面把常用的命令保存下来方便查看，里面的一些名词不一定用的准确。

- 创建sshkey

    ```bash
    ssh-keygen -t rsa -C "youremail@example.com"
    ```

- 基本命令（创建，添加）

    command|comments
    :-:|:-:
    git init | 初始化仓库
    git add | 添加文件到stage
    git status | 查看当前工作区状态
    git commit -m "xxx" | 提交修改到版本库

- 版本控制

    command|comments
    :-:|:-:
    git log | 查看历史commit
    git log --graph | 查看历史commit，可以看到分支合并过程
    git diff <filename> | 查看修改
    git reset --hard HEAD^(HEAD~100) | 版本回退
    git reset --hard <commit_id> | 版本回退
    git reflog | 打印每一次commit号
    git check -- <file> | 清空*工作区*的修改
    git reset HEAD <file> | unstage, 即把stage中的修改放到工作区去

- 创建与合并分支

    command|comments
    :-: | :-:
    git checkout -b <dev> | 创建并转到分支
    git branch | 查看当前分支状态
    git branch <dev> | 创建分支
    git checkout <dev> | 转到分支
    git merge <dev> | 合并分支
    git merge --no-ff <dev> | 合并分支，并且不以ff模式合并
    git branch -d(D) <dev> | 删除分支

- 存储工作现场

    command | comments
    :-: | :-:
    git stash | 存储当前工作现场，清空工作区以做别的事情
    git stash list | 查看当前stash列表
    git stash pop | 恢复stash内容并删除
	
## 参考资料

- [git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
