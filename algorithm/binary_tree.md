---
layout: page
---

<!-- vim-markdown-toc Marked -->

* [144二叉树的前序遍历](#144二叉树的前序遍历)
* [94二叉树的中序遍历](#94二叉树的中序遍历)
* [226翻转二叉树](#226翻转二叉树)
* [101对称二叉树](#101对称二叉树)
* [104二叉树的最大深度](#104二叉树的最大深度)
* [111二叉树的最小深度](#111二叉树的最小深度)
* [222完全二叉树的节点个数](#222完全二叉树的节点个数)
* [110平衡二叉树](#110平衡二叉树)
* [257二叉树的所有路径](#257二叉树的所有路径)
* [总结与思考](#总结与思考)

<!-- vim-markdown-toc -->


## 144二叉树的前序遍历

**链接**

[https://leetcode.cn/problems/binary-tree-preorder-traversal/](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

**思路**

- 代码随想录
- [史上最全遍历二叉树详解](https://leetcode.cn/problems/binary-tree-preorder-traversal/solution/leetcodesuan-fa-xiu-lian-dong-hua-yan-shi-xbian-2/)

**代码**

**递归**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* cur, vector<int>& result) {
        if (cur == NULL) return;
        result.push_back(cur->val);
        dfs(cur->left, result);
        dfs(cur->right, result);
    }

    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        dfs(root, result);
        return result;
    }
};
```

**迭代**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) return {}; //特判

        stack<TreeNode*> m_stack;
        m_stack.push(root); //根节点入栈
        while (!m_stack.empty()) {
            //处理当前栈顶
            TreeNode* cur = m_stack.top();
            result.push_back(cur->val);
            m_stack.pop();

            //非空节点入栈，先右后左，这样出来时候是先左后右
            if (cur->right) m_stack.push(cur->right);
            if (cur->left) m_stack.push(cur->left);
        }
        return result;
    }
};
```


## 94二叉树的中序遍历

**链接**

[https://leetcode.cn/problems/binary-tree-inorder-traversal/](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

**思路**

- 代码随想录
- [史上最全遍历二叉树详解](https://leetcode.cn/problems/binary-tree-preorder-traversal/solution/leetcodesuan-fa-xiu-lian-dong-hua-yan-shi-xbian-2/)

**代码**

**递归**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void mid_order(TreeNode* cur, vector<int>& result) {
        if (cur == nullptr) return;
        mid_order(cur->left, result);
        result.push_back(cur->val);
        mid_order(cur->right, result); 
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        if (!root) return result;

        mid_order(root, result);
        return result;
    }
};
```

**迭代**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        if (!root) return result;

        stack<TreeNode*> m_stack;
        TreeNode* cur = root;
        while (!m_stack.empty() || cur != nullptr) {
            // if (cur != nullptr) {
            //     m_stack.push(cur);
            //     cur = cur->left;
            // } else {
            //     cur = m_stack.top();
            //     m_stack.pop();
            //     result.push_back(cur->val);
            //     cur = cur->right;
            // }

            //用while循环做
            while (cur != nullptr) {
                m_stack.push(cur);
                cur = cur->left;
            }
            cur = m_stack.top();
            m_stack.pop();
            result.push_back(cur->val);
            cur = cur->right;
        }
        return result;
    }
};
```

## 226翻转二叉树

**链接**

[https://leetcode.cn/problems/invert-binary-tree/](https://leetcode.cn/problems/invert-binary-tree/)

**思路**

- 代码随想录

**代码**

**递归**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void swap_nodes(TreeNode* root) {
        if (root == nullptr) return;
        swap(root->left, root->right);
        swap_nodes(root->left);
        swap_nodes(root->right);
    }

    TreeNode* invertTree(TreeNode* root) {
        swap_nodes(root);
        return root;
    }
};
```

**迭代**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) return nullptr;

        stack<TreeNode*> m_stack;
        m_stack.push(root);
        while (!m_stack.empty()) {
            TreeNode* cur = m_stack.top();
            m_stack.pop();
            swap(cur->left, cur->right);

            //注意空指针不放入，总是忘
            if (cur->right) m_stack.push(cur->right);
            if (cur->left) m_stack.push(cur->left);
        }

        return root;
    }
};
```

## 101对称二叉树

**链接**

[https://leetcode.cn/problems/symmetric-tree/](https://leetcode.cn/problems/symmetric-tree/)

**思路**

- 代码随想录

**代码**

**递归:** 根据代码随想录和官方解析融合解法

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool compare(TreeNode* left, TreeNode* right) {
        //判断空指针的情况
        if (!left && !right) return true;
        if (!left || !right) return false;

        bool is_equal = left->val == right->val; //当前节点是否相等
        bool outside = compare(left->left, right->right); //下一层外侧节点
        bool inside = compare(left->right, right->left); //下一层内侧节点
        return is_equal && outside && inside; //任何一处为false都得返回false
    }

    bool isSymmetric(TreeNode* root) {
        return compare(root, root);
    }
};
```

## 104二叉树的最大深度

**链接**

[https://leetcode.cn/problems/maximum-depth-of-binary-tree/](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

**思路**

- 代码随想录

**代码**

**递归**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int get_depth(TreeNode* node) {
        if (node == nullptr) return 0;
        return 1 + max(get_depth(node->left), get_depth(node->right));
    }

    int maxDepth(TreeNode* root) {
        return get_depth(root);
    }
};
```


## 111二叉树的最小深度

**链接**

[https://leetcode.cn/problems/minimum-depth-of-binary-tree/](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

**思路**

- 代码随想录

**递归代码**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (!root) return 0; //根节点为空
        if (root->left && !root->right) { //左不空右空
            return minDepth(root->left) + 1;
        }
        if (!root->left && root->right) { //左空右不空
            return minDepth(root->right) + 1;
        }
        //左右都空或者左右都不空
        return 1 + min(minDepth(root->left), minDepth(root->right));
    }
};
```

## 222完全二叉树的节点个数

**链接**

[https://leetcode.cn/problems/count-complete-tree-nodes/](https://leetcode.cn/problems/count-complete-tree-nodes/)

**思路**

- 代码随想录

**代码**

深度优先，前序遍历

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* node, int& result) {
        if (!node) return;
        result += 1;
        dfs(node->left, result);
        dfs(node->right, result);
    }

    int countNodes(TreeNode* root) {
        if (!root) return 0;
        //深度优先遍历，前序遍历
        int result = 0;
        dfs(root, result);
        return result;
    }
};
```

广度优先遍历：

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (!root) return 0;
        // 广度优先，时间复杂度为n 
        int result = 0;
        queue<TreeNode*> m_queue;
        m_queue.push(root);
        while (!m_queue.empty()) {
            TreeNode* cur = m_queue.front();
            m_queue.pop();
            result += 1;
            if (cur->left) m_queue.push(cur->left);
            if (cur->right) m_queue.push(cur->right);
        }
        return result;
    }
};
```

降低时间复杂度

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (!root) return 0;

        //时间复杂度为log(n)^2算法
        TreeNode* left = root->left;
        TreeNode* right = root->right;
        int left_height = 0;
        int right_height = 0;

        while (left) {
            left = left->left;
            left_height += 1;
        }
        while (right) {
            right = right->right;
            right_height += 1;
        }
        if (left_height == right_height) {//如果是满二叉树，则用公式计算
            return (2 << left_height) - 1;
        }
        //不是满二叉树，左右分别遍历，直到为满二叉树为止
        return countNodes(root->left) + countNodes(root->right) + 1;

    }
};
```


## 110平衡二叉树

**链接**

[https://leetcode.cn/problems/balanced-binary-tree/](https://leetcode.cn/problems/balanced-binary-tree/)

**思路**

- 代码随想录

**代码**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:

    //后序遍历，求节点高度
    int get_depth(TreeNode* node) {
        if (!node) return 0; //终止条件

        //-1定义为当前节点不是平衡二叉树
        int left_depth = get_depth(node->left);
        if (left_depth == -1) return -1;
        int right_depth = get_depth(node->right);
        if (right_depth == -1) return -1;

        if (abs(left_depth - right_depth) > 1) {
            return -1;
        } else {
            return 1 + max(left_depth, right_depth);
        }
    }

    bool isBalanced(TreeNode* root) {
        return get_depth(root) == -1 ? false : true;
    }
};
```

## 257二叉树的所有路径

**链接**

[https://leetcode.cn/problems/binary-tree-paths/](https://leetcode.cn/problems/binary-tree-paths/)

**思路**

- 代码随想录

**代码**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void find_path(TreeNode* node, string path, vector<string>& result) {
        if (!node) return; //终止条件

        path += to_string(node->val); //中
        if (!node->left && !node->right) { //终止条件
            result.push_back(path);
            return;
        }
        //左右继续
        path += "->";
        find_path(node->left, path, result);
        find_path(node->right, path, result);
    }

    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        find_path(root, "", result);
        return result;
    }
};
```

## 总结与思考

- 递归三部曲: 
    - 函数参数
    - 终止条件
    - 单层循环逻辑
- 尽可能减少使用递归，换用迭代，防止递归过深出现问题
