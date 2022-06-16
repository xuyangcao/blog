---
layout: page
id: backtracking
---

<!-- vim-markdown-toc Marked -->

* [前言](#前言)
* [题目列表](#题目列表)
* [思考与总结](#思考与总结)
* [参考资料](#参考资料)

<!-- vim-markdown-toc -->

## 前言

回溯算法的本质是对树型或者图型结构进行一次深度优先搜索，是一种暴力搜索算法，因此其时间复杂度是较高的，为指数级别。

## 题目列表

| 题目 | 链接 | 
|:-----|:-----|
| 77. 组合 | [https://leetcode.cn/problems/combinations/](https://leetcode.cn/problems/combinations/) |
| 39. 组合总和 | [https://leetcode.cn/problems/combination-sum/](https://leetcode.cn/problems/combination-sum/) |
| 40. 组合总和 II | [https://leetcode.cn/problems/combination-sum-ii/](https://leetcode.cn/problems/combination-sum-ii/) |
| 216. 组合总和 III | [https://leetcode.cn/problems/combination-sum-iii/](https://leetcode.cn/problems/combination-sum-iii/) |
| 17. 电话号码的字母组合 | [https://leetcode.cn/problems/letter-combinations-of-a-phone-number/](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/) |
| 131. 分割回文串 | [https://leetcode.cn/problems/palindrome-partitioning/](https://leetcode.cn/problems/palindrome-partitioning/) |
| 93. 复原 IP 地址 | [https://leetcode.cn/problems/restore-ip-addresses/](https://leetcode.cn/problems/restore-ip-addresses/) |
| 78. 子集 | [https://leetcode.cn/problems/subsets/](https://leetcode.cn/problems/subsets/) |
| 90. 子集 II | [https://leetcode.cn/problems/subsets-ii/](https://leetcode.cn/problems/subsets-ii/) |
| 491. 递增子序列 | [https://leetcode.cn/problems/increasing-subsequences/](https://leetcode.cn/problems/increasing-subsequences/) |
| 46. 全排列 | [https://leetcode.cn/problems/permutations/](https://leetcode.cn/problems/permutations/) |
| **47. 全排列 II** | [https://leetcode.cn/problems/permutations-ii/](https://leetcode.cn/problems/permutations-ii/) |
| **51. N 皇后** | [https://leetcode.cn/problems/n-queens/](https://leetcode.cn/problems/n-queens/) |
| **37. 解数独** | [https://leetcode.cn/problems/sudoku-solver/](https://leetcode.cn/problems/sudoku-solver/) |
| 22. 括号生成 | [https://leetcode.cn/problems/generate-parentheses/](https://leetcode.cn/problems/generate-parentheses/) |

## 思考与总结

从题目列表来看，几乎所有的回溯算法全部可以用一个树形图来帮助我们设计程序。只有深刻理解如何绘制树形图，才能设计出回溯算法，并做出相应的**剪枝**操作来提升算法的效率。

代码随想录中总结了一套“回溯三部曲”较为实用，如下:

1. 函数参数及返回值
2. 终止条件
3. 单层循环逻辑

其中还提供了一个回溯算法的代码模板，学习前期可以根据这个模板感受回溯算法的表象，慢慢地再深刻理解其本质，具体内容见参考资料[1]。

根据题目列表，可以将回溯算法简单分为如下几类[1]:

- 组合问题
- 分割问题
- 子集问题
- 排列问题
- 棋盘问题

在**排列问题**和**组合问题**中，要深刻理解去重的原理。


## 参考资料

[1] [代码随想录](https://programmercarl.com/)

[2] [https://leetcode.cn/tag/backtracking/problemset/](https://leetcode.cn/tag/backtracking/problemset/)
