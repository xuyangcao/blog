---
layout: page
---

<!-- vim-markdown-toc Marked -->

* [242. 有效的字母异位词](#242.-有效的字母异位词)
* [349. 两个数组的交集](#349.-两个数组的交集)
* [202. 快乐数](#202.-快乐数)
* [383. 赎金信](#383.-赎金信)
* [1. 两数之和](#1.-两数之和)
* [454. 四数相加 II](#454.-四数相加-ii)
* [15.三数之和](#15.三数之和)
* [18. 四数之和](#18.-四数之和)
* [思考与总结](#思考与总结)

<!-- vim-markdown-toc -->


## [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

**思路**

见代码随想录


**代码**

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()) {
            return false;
        }

        int dict[26] = {0};
        for (int i = 0; i < s.size(); i++) {
            dict[s[i] - 'a']++;
        }
        for (int i = 0; i < t.size(); i++) {
            dict[t[i] - 'a']--;
        }
        for (int i = 0; i < 26; i++) {
            if (dict[i] != 0) {
                return false;
            }
        }
        return true;
    }
};
```



## [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> mem_set(nums1.begin(), nums1.end());
        unordered_set<int> nums2_(nums2.begin(), nums2.end());

        vector<int> result;
        for (int num : nums2_) {
            if(mem_set.find(num) != mem_set.end()) {
                result.push_back(num);
            }
        }
        return result;
    }
};
```

## [202. 快乐数](https://leetcode.cn/problems/happy-number/)

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    int GetSum(int num) {
        int sum = 0;
        int res = 0;
        while (num) {
            res = num % 10;
            sum += res * res;
            num /= 10;
        }
        return sum;
    }

    bool isHappy(int n) {
        unordered_set<int> mem;
        int sum = GetSum(n);
        while (1) {
            if (sum == 1) { //如果为1，则返回真
                return true;
            } else if (mem.find(sum) != mem.end()) { //如果在集合中出现了，说明循环了
                return false;
            } else { //否则将结果加入集合，并继续计算sum
                mem.insert(sum);
                sum = GetSum(sum);
            }
        }
    }
};
```

## [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int dic[26] = {0};
        for (int i = 0; i < magazine.size(); i++) {
            dic[magazine[i] - 'a']++;
        }
        for (int i = 0; i < ransomNote.size(); i++) {
            dic[ransomNote[i] - 'a']--;

            if (dic[ransomNote[i] - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```


## [1. 两数之和](https://leetcode.cn/problems/two-sum/)

**思路**

见代码随想录


**代码**

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> umap;
        vector<int> result;
        for (int i = 0; i < nums.size(); i++) {
            if (umap.find(target - nums[i]) != umap.end()) {
                result.push_back(umap.find(target - nums[i])->second);
                result.push_back(i);
                return result;
            }
            umap.insert(pair<int, int>(nums[i], i));
        }
        return result;
    }
};
```

## [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> umap_ab;
        int count = 0;
        for (int a : nums1) {
            for (int b : nums2) {
                umap_ab[a + b]++;
            }
        }
        for (int c : nums3) {
            for (int d : nums4) {
                if (umap_ab.find(0 - c - d) != umap_ab.end()) {
                    count += umap_ab[- c - d];
                }
            }
        }
        return count;
    }
};
```


## [15.三数之和](https://leetcode.cn/problems/3sum/)

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

## [18. 四数之和](https://leetcode.cn/problems/4sum/)

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

## 思考与总结

- 一般的，如果仅仅需要将26个英文字母存入hash table，则可以考虑用数组的形式，因为这样增删和查询速度更快。如 [242. 有效的字母异位词](#242.-有效的字母异位词) 和 [383. 赎金信](#383.-赎金信)

- 当大数相加会导致潜在的溢出时，可以考虑相减的形式，例如 [18. 四数之和](#18.-四数之和)。

- 常用: 求一个数各个位数上的和，如下:

    ```cpp
    int GetSum(int num) {
        int sum = 0;
        int res = 0;
        while (num) {
            res = num % 10;
            sum += res * res;
            num /= 10;
        }
        return sum;
    }
    ```
