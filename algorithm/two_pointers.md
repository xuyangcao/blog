---
layout: page
---

<!-- vim-markdown-toc Marked -->

* [27移除元素](#27移除元素)
* [344反转字符串](#344反转字符串)
* [剑指Offer05替换空格](#剑指offer05替换空格)
* [206反转链表](#206反转链表)
* [19删除链表的倒数第N个结点](#19删除链表的倒数第n个结点)
* [142环形链表II](#142环形链表ii)
* [15三数之和](#15三数之和)
* [18四数之和](#18四数之和)
* [总结与思考](#总结与思考)
* [参考资料](#参考资料)

<!-- vim-markdown-toc -->

## 27移除元素

**链接**

https://leetcode.cn/problems/remove-element/

**思路**

见代码随想录

快慢指针

**代码**

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow_index = 0;
        for (int fast_index = 0; fast_index < nums.size(); fast_index++) {
            if (nums[fast_index] != val) {
                nums[slow_index] = nums[fast_index];
                slow_index += 1; //替换完之后，慢指针前移一步
            }
        }
        //注意这里没有加1，因为第8行已经加过了，此时指向的内容还没经过确认
        return slow_index; 
    }
};
```

## 344反转字符串

**链接**

https://leetcode.cn/problems/reverse-string/

**思路**

见代码随想录

左右指针

**代码**

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        /*双指针 */
        int left = 0; 
        int right = s.size() - 1;
        while (left < right) {
            swap(s[left], s[right]);
            left++;
            right--;
        }
    }
};
```

## 剑指Offer05替换空格

**链接**

https://leetcode.cn/problems/ti-huan-kong-ge-lcof/

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    string replaceSpace(string s) {
        string result;
        for (char str : s) {
            if (str == ' ') {
                result += "%20";
                continue;
            }
            result += str;
        }
        return result;
    }
};
```

## 206反转链表

**链接**

https://leetcode.cn/problems/reverse-linked-list/

**思路**

见代码随想录

**代码**

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = NULL;
        ListNode* cur = head;

        ListNode* temp = NULL;
        while (cur) {
           temp = cur->next; 
           cur->next = pre;
           pre = cur;
           cur = temp;
        }
        return pre;
    }
};
```

## 19删除链表的倒数第N个结点

**链接**

https://leetcode.cn/problems/remove-nth-node-from-end-of-list/

**思路**

见代码随想录

**代码**

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy_head = new ListNode(0);
        dummy_head->next = head;
        ListNode* left = dummy_head;
        ListNode* right = head;

        for (int i = 0; i < n; i++) {
            right = right->next;
        }
        while (right) {
            right = right->next;
            left = left->next;
        }
        left->next = left->next->next;
        head = dummy_head->next;
        delete dummy_head;
        return head;
    }
};
```

## 142环形链表II

**链接**

https://leetcode.cn/problems/linked-list-cycle-ii/

**思路**

见代码随想录

**代码**

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (!head) {
            return NULL;
        }
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast->next != NULL && fast->next->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            if (fast == slow) {
                ListNode* left = head;
                ListNode* right = fast;
                while (left != right) {
                    left = left->next;
                    right = right->next;
                }
                return right;
            }
        }
       return NULL; 
    }
};
```

## 15三数之和

**链接**

https://leetcode.cn/problems/3sum/

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    /*
    这道题整体思路是两重循环，第一重循环确定一个数，第二重循环通过双指针确定后两个数；
    若三数之和>0，则right--；
    若三数之和<0,则left++；
    剩下的就是如何处理重复数组的一些trick。
    */
    vector<vector<int>> threeSum(vector<int>& nums) {
        int length = nums.size();
        if (length < 3) return {}; //特判

        sort(nums.begin(), nums.end()); //排序
        vector<vector<int>> result; // 存放最终结果
        for (int i = 0; i < length; i++) { // 固定第一个数，转化为求两数之和
            //第一个数>0,后面的全大于0，不需要继续了
            if (nums[i] > 0) return result; 
            //去重，从nums[1]开始，如果和前一个相等，则跳过，避免结果重复
            if (i > 0 && nums[i] == nums[i-1]) continue; 
            
            //left和right指针
            int left = i + 1;
            int right = length - 1;
            while (left < right) {
                if (nums[i] + nums[left] + nums[right] > 0) {
                    right--; //三数之和太大，右指针左移
                } else if (nums[i] + nums[left] + nums[right] < 0) {
                    left++; // 三数之和太小，左指针右移
                } else { // 找到一个的三元组，添加到结果中，左右指针内缩，继续寻找
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    right--;
                    left++;
                    //去重，第二个数和第三个数也不重复
                    //注意这里left<right的条件不能丢失
                    while (left < right && nums[right] == nums[right + 1]) right--;
                    while (left < right && nums[left] == nums[left - 1]) left++;
                }
            }
        }
        return result;
    }
};
```

## 18四数之和

**链接**

https://leetcode.cn/problems/4sum/

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        int length = nums.size();
        if (length < 4) return {}; //特判

        vector<vector<int>> res; //结果
        sort(nums.begin(), nums.end()); //排序
        for (int i = 0; i < length; i++) { //第一个数
            if (i > 0 && nums[i] == nums[i-1]) continue; //去重
            for (int j = i + 1; j < length; j++) { //第二个数,注意从i+1开始
                if (j > i + 1 && nums[j] == nums[j-1]) continue; //去重,注意i+1
                int left = j + 1; //第三个数，left
                int right = length - 1; //第四个数,right
                
                while (left < right) {
                    // nums[k] + nums[i] + nums[left] + nums[right] > target 会溢出
                    if (nums[i] + nums[j] > target - nums[left] - nums[right]) {
                        right--; //四数之和大于target, right--
                    } else if (nums[i] + nums[j] < target - nums[left] - nums[right]) {
                        left++; //四数之和小于target,left++
                    } else { //找到结果，保存，双指针内缩，最后去重
                        res.push_back(vector<int>{nums[i], nums[j], nums[left], nums[right]});
                        left++;
                        right--;
                        while (left < right && nums[left] == nums[left - 1]) left++;
                        while (left < right && nums[right] == nums[right + 1]) right--;
                    }
                }
            }
        }
        return res;
    }
};
```

## 总结与思考


## 参考资料

- [Leetcode: 双指针](https://leetcode.cn/tag/two-pointers/problemset/)
