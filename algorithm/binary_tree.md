---
layout: post 
---

<!-- vim-markdown-toc Marked -->

* [前言](#前言)
* [题目列表](#题目列表)
* [思考与总结](#思考与总结)
* [参考资料](#参考资料)

<!-- vim-markdown-toc -->

## 前言

二叉树是学习递归的优秀数据结构，对于二叉树问题的求解，关键在于对递归算法和不同二叉树的属性有深刻的理解。


## 题目列表

| 题目 | 链接 | 
|:-----|:-----|
| 144二叉树的前序遍历 | [https://leetcode.cn/problems/binary-tree-preorder-traversal/](https://leetcode.cn/problems/binary-tree-preorder-traversal/) |
| 94二叉树的中序遍历 | [https://leetcode.cn/problems/binary-tree-inorder-traversal/](https://leetcode.cn/problems/binary-tree-inorder-traversal/) |
| 145二叉树的后序遍历 | [https://leetcode.cn/problems/binary-tree-postorder-traversal/](https://leetcode.cn/problems/binary-tree-postorder-traversal/) |
| 102二叉树的层序遍历 | [https://leetcode.cn/problems/binary-tree-level-order-traversal/](https://leetcode.cn/problems/binary-tree-level-order-traversal/) |
| 226翻转二叉树 | [https://leetcode.cn/problems/invert-binary-tree/](https://leetcode.cn/problems/invert-binary-tree/) |
| 101对称二叉树 | [https://leetcode.cn/problems/symmetric-tree/](https://leetcode.cn/problems/symmetric-tree/) |
| 104二叉树的最大深度 | [https://leetcode.cn/problems/maximum-depth-of-binary-tree/](https://leetcode.cn/problems/maximum-depth-of-binary-tree/) |
| 111二叉树的最小深度 | [https://leetcode.cn/problems/minimum-depth-of-binary-tree/](https://leetcode.cn/problems/minimum-depth-of-binary-tree/) |
| 222完全二叉树的节点个数 | [https://leetcode.cn/problems/count-complete-tree-nodes/](https://leetcode.cn/problems/count-complete-tree-nodes/) |
| **110平衡二叉树** | [https://leetcode.cn/problems/balanced-binary-tree/](https://leetcode.cn/problems/balanced-binary-tree/) |
| **257二叉树的所有路径** | [https://leetcode.cn/problems/binary-tree-paths/](https://leetcode.cn/problems/binary-tree-paths/) |
| 404左叶子之和 | [https://leetcode.cn/problems/sum-of-left-leaves/](https://leetcode.cn/problems/sum-of-left-leaves/) |
| 513找树左下角的值 | [https://leetcode.cn/problems/find-bottom-left-tree-value/](https://leetcode.cn/problems/find-bottom-left-tree-value/) | 
| 112路径总和 | [https://leetcode.cn/problems/path-sum/](https://leetcode.cn/problems/path-sum/) |
| **113路径总和II** | [https://leetcode.cn/problems/path-sum-ii/](https://leetcode.cn/problems/path-sum-ii/) |
| **106从中序与后序遍历序列构造二叉树** | [https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/) |
| 105从前序与中序遍历序列构造二叉树 | [https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) |
| 654最大二叉树 | [https://leetcode.cn/problems/maximum-binary-tree/](https://leetcode.cn/problems/maximum-binary-tree/) |
| 617合并二叉树 | [https://leetcode.cn/problems/merge-two-binary-trees/](https://leetcode.cn/problems/merge-two-binary-trees/) |
| 700二叉搜索树中的搜索 | [https://leetcode.cn/problems/search-in-a-binary-search-tree/](https://leetcode.cn/problems/search-in-a-binary-search-tree/) |
| **98验证二叉搜索树** | [https://leetcode.cn/problems/validate-binary-search-tree/](https://leetcode.cn/problems/validate-binary-search-tree/) |
| **530二叉搜索树的最小绝对差** | [https://leetcode.cn/problems/minimum-absolute-difference-in-bst/](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)  |
| 501二叉搜索树中的众数 | [https://leetcode.cn/problems/find-mode-in-binary-search-tree/](https://leetcode.cn/problems/find-mode-in-binary-search-tree/) |
| **236二叉树的最近公共祖先** | [https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/) |
| 235二叉搜索树的最近公共祖先 | [https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/) | 
| 701二叉搜索树中的插入操作 | [https://leetcode.cn/problems/insert-into-a-binary-search-tree/](https://leetcode.cn/problems/insert-into-a-binary-search-tree/) |
| **450删除二叉搜索树中的节点** | [https://leetcode.cn/problems/delete-node-in-a-bst/](https://leetcode.cn/problems/delete-node-in-a-bst/)|
| **669修剪二叉搜索树** | [https://leetcode.cn/problems/trim-a-binary-search-tree/](https://leetcode.cn/problems/trim-a-binary-search-tree/) |
| 108将有序数组转换为二叉搜索树 | [https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/) |
| 538把二叉搜索树转换为累加树 | [https://leetcode.cn/problems/convert-bst-to-greater-tree/](https://leetcode.cn/problems/convert-bst-to-greater-tree/) |

## 思考与总结

整个二叉树一节内的所有题目基本由**二叉树的遍历**这个主题展开，并且由前序，中序，后序和层序遍历来求解。对于一个新问题，首先可以想想能够用什么遍历方式来解决。

前中后序均可由递归来解决，递归三部曲可总结如下: 

1. 函数参数
2. 终止条件
3. 单层循环逻辑

关于递归函数什么时候需要返回值，代码随想录里面给了三个规律性的准则，这里我进一步将其缩减为一句话： **如果需要处理递归返回值，则需要返回值，后序遍历更方便处理递归返回值。** 代码随想录中的三条准则如下：

- 如果需要搜索整棵二叉树且不用处理递归返回值，递归函数就不要返回值。
- 如果需要搜索整棵二叉树且需要处理递归返回值，递归函数就需要返回值。
- 如果要搜索其中一条符合条件的路径，那么递归一定需要返回值，因为遇到符合条件的路径了就要及时返回。

前序，中序，后序遍历指处理根节点在前，中、后的位置。可以发现，前序遍历就是在**进入当前中间节点**时对数据进行处理；中序遍历就是在**离开左节点后进入右节点之前**对数据进行处理；而后序遍历就是在**离开当前中间节点之前**对数据进行处理。特别需要注意的是，**这里的处理操作可简单可复杂，简单的如将当前中间节点加入vector中，复杂起来可以在当前位置做各种各样复杂的操作。**

根据上一点的描述，后序遍历是先处理左节点和右节点，然后再处理当前中间节点，根据这一性质，**可以巧妙设计递归的返回值，在处理中间节点时利用子节点返回来的信息**。这一点和动态规划中的核心思想非常相像，即**把一个大问题分解成若干个子问题，然后将子问题整合为大问题的解**。我们可以发现如下三个问题的核心思想非常相近，即*分而治之*：

- 后序遍历
- 归并排序
- 动态规划


根据中序和后序结果构造二叉树一题中(#106)，其关键是要**充分挖掘二叉树遍历结果的性质**。挖掘到如下性质后我们就不难做题了：

1. 在后序遍历结果中，最后一个元素一定是根节点。（但后序遍历却无法分辨出左右子树的分界点，因此必须借助中序遍历结果）。**根据这个性质,我们就可以找到每个递归过程的中间节点。**

2. 在中序遍历结果中，根节点一旦确定，其左边为左子树的所有元素，右边为右子树的所有元素。**根据这个性质，可以将中序遍历拆分为 [左子树元素，中间节点，右子树元素]。**

3. (这条不易察觉)由于中序遍历过程为“左->中->右”，后序遍历过程为"左->右->中"，根据这个过程可以发现，一旦我们根据中序遍历结果确定了左子树的长度和元素，只需在后序遍历结果中从头截取对应长度即可得到后序遍历的左子树所有元素。**根据这个性质，可以将后序遍历结果拆分为 [左子树元素，右子树元素，中间节点]。**

4. 结合1，2，3点，我们能够找到中间节点，并分别在中序和后序遍历中将左子树和右子树的所有元素拆分出来，接下来分别在左侧和右侧利用中序和后序的两个子树内元素不断递归即可。参考资料[4]中给出了不错的图解，可以参考。

在利用数组构建二叉树时，每次将一个数组拆分为左子树元素和右子树元素时尽量不要定义新数组，而是采用下标索引的方式更优，因为这样可以节省时间和空间成本。(如106和654题)

在处理二叉搜索树时，需要充分考虑其特性，即左子树均小于中间节点，右子树均大于中间节点。因此，如果使用中序遍历，则整个树遍历出来就是从小到大排列的，类似于在处理有序数组，但也不是所有问题必须用中序遍历来处理。代码随想录中提供了一种使用pre指针处理相邻节点的操作，非常实用，建议掌握(98, 530题)。此外，二叉树的搜索一般是有方向的，因为数据的排列是有序的（#701）。

在实际项目开发中，尽可能减少使用递归，因为项目参数和调用关系较为复杂，不易控制递归深度[1]。



## 参考资料

[1] [代码随想录](https://programmercarl.com/)

[2] [史上最全遍历二叉树详解](https://leetcode.cn/problems/binary-tree-preorder-traversal/solution/leetcodesuan-fa-xiu-lian-dong-hua-yan-shi-xbian-2/)

[3] [labuladong 的算法网站](https://labuladong.gitee.io/algo/)

[4] [图解构造二叉树之中序+后序](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/solution/tu-jie-gou-zao-er-cha-shu-wei-wan-dai-xu-by-user72/)
