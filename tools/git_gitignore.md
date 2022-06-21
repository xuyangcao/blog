---
layout: post
---

## 问题

每次git status时会出来一大堆不想add的文件（夹），很烦

## 解决方法

- 添加[.gitignore]([https://github.com/github/gitignore](https://github.com/github/gitignore)
)文件到当前目录即可。 

- 创建git仓库时可选择直接初始化.gitignore

- 可以根据自己的需求修改`.gitignore`文件。 改文件的部分内容如下，如我想停止追踪当前工程下的`runs`文件夹，在`.gitignore`文件中添加最后两行内容即可：

```bash
103
104 # Environments
105 .env
106 .venv
107 env/
108 venv/
109 ENV/
110 env.bak/
111 venv.bak/
112
113 # Spyder project settings
114 .spyderproject
115 .spyproject
116
117 # Rope project settings
118 .ropeproject
119
120 # mkdocs documentation
121 /site
122
123 # mypy
124 .mypy_cache/
125 .dmypy.json
126 dmypy.json
127
128 # Pyre type checker
129 .pyre/
130
131
132 # added by xuyangcao
133 /runs
```
