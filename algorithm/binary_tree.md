---
layout: page
---

<!-- vim-markdown-toc Marked -->

* [前言](#前言)
* [题目列表](#题目列表)
    * [二叉树的遍历](#二叉树的遍历)
    * [二叉树的属性](#二叉树的属性)
    * [二叉树的修改与构造](#二叉树的修改与构造)
* [思考与总结](#思考与总结)
* [参考资料](#参考资料)

<!-- vim-markdown-toc -->

## 前言


## 题目列表

### 二叉树的遍历

| 题目 | 链接 | 
|:-----|:-----|
| 144二叉树的前序遍历 | [https://leetcode.cn/problems/binary-tree-preorder-traversal/](https://leetcode.cn/problems/binary-tree-preorder-traversal/) |
| 94二叉树的中序遍历 | [https://leetcode.cn/problems/binary-tree-inorder-traversal/](https://leetcode.cn/problems/binary-tree-inorder-traversal/) |
| 145二叉树的后序遍历 | [https://leetcode.cn/problems/binary-tree-postorder-traversal/](https://leetcode.cn/problems/binary-tree-postorder-traversal/) |
 

### 二叉树的属性

| 题目 | 链接 | 
|:-----|:-----|
| 101对称二叉树 | [https://leetcode.cn/problems/symmetric-tree/](https://leetcode.cn/problems/symmetric-tree/) |
| 104二叉树的最大深度 | [https://leetcode.cn/problems/maximum-depth-of-binary-tree/](https://leetcode.cn/problems/maximum-depth-of-binary-tree/) |
| 111二叉树的最小深度 | [https://leetcode.cn/problems/minimum-depth-of-binary-tree/](https://leetcode.cn/problems/minimum-depth-of-binary-tree/) |
| 222完全二叉树的节点个数 | [https://leetcode.cn/problems/count-complete-tree-nodes/](https://leetcode.cn/problems/count-complete-tree-nodes/) |
| **110平衡二叉树** | [https://leetcode.cn/problems/balanced-binary-tree/](https://leetcode.cn/problems/balanced-binary-tree/) |
| **257二叉树的所有路径** | [https://leetcode.cn/problems/binary-tree-paths/](https://leetcode.cn/problems/binary-tree-paths/) |

### 二叉树的修改与构造

| 题目 | 链接 | 
|:-----|:-----|
| 226翻转二叉树 | [https://leetcode.cn/problems/invert-binary-tree/](https://leetcode.cn/problems/invert-binary-tree/) |


## 思考与总结

递归三部曲: 

1. 函数参数
2. 终止条件
3. 单层循环逻辑

前序，中序，后序遍历指处理根节点在前，中、后的位置。可以发现，前序遍历就是在**进入当前中间节点**时对数据进行处理；中序遍历就是在**离开左节点后进入右节点之前**对数据进行处理；而后序遍历就是在**离开当前中间节点之前**对数据进行处理。

根据上一点的描述，后序遍历是先处理左节点和右节点，然后再处理当前中间节点，根据这一性质，**可以巧妙设计递归的返回值，在处理中间节点时利用子节点返回来的信息**。这一点和动态规划中的核心思想非常相像，即**把一个大问题分解成若干个子问题，然后将子问题整合为大问题的解**。我们可以发现如下三个问题的核心思想非常相近，即*分而治之*：

- 后序遍历
- 归并排序
- 动态规划

尽可能减少使用递归，换用迭代，防止递归过深出现问题



## 参考资料

- [代码随想录](https://programmercarl.com/)
- [史上最全遍历二叉树详解](https://leetcode.cn/problems/binary-tree-preorder-traversal/solution/leetcodesuan-fa-xiu-lian-dong-hua-yan-shi-xbian-2/)
- [labuladong 的算法网站](https://labuladong.gitee.io/algo/)
