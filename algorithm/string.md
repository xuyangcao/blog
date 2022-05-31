---
layout: page
---

<!-- vim-markdown-toc Marked -->

* [344反转字符串](#344反转字符串)
* [541反转字符串II](#541反转字符串ii)
* [剑指Offer05替换空格](#剑指offer05替换空格)
* [151颠倒字符串中的单词](#151颠倒字符串中的单词)
* [剑指Offer58-II左旋转字符串](#剑指offer58-ii左旋转字符串)
* [28实现strStr()](#28实现strstr())
* [459重复的子字符串](#459重复的子字符串)
* [总结与思考](#总结与思考)
* [参考资料](#参考资料)

<!-- vim-markdown-toc -->


## 344反转字符串

**链接**

https://leetcode.cn/problems/reverse-string/

**思路**

见代码随想录

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

## 541反转字符串II

**链接**

https://leetcode.cn/problems/reverse-string-ii/

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        int n = s.size();
        for (int i = 0; i < n; i += (2 * k)) {
            reverse(s.begin() + i, s.begin() + min(i + k, n));
        }
        return s;
    }
};
```

## 剑指Offer05替换空格

**链接**

https://leetcode.cn/problems/ti-huan-kong-ge-lcof/

**思路**

见代码随想录

**代码**

这里我的实现方法使用了额外的空间复杂度

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

## 151颠倒字符串中的单词

**链接**

https://leetcode.cn/problems/reverse-words-in-a-string/

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    string reverseWords(string s) {
        //移除多余空格
        int length = s.size();
        //注意这里逆序遍历，防止中间的空格去除不干净
        for (int i = length; i > 0; i--) {
            if (s[i] == s[i - 1] && s[i] == ' ') {
                s.erase(s.begin() + i);
            }
        }
        if (s.size() > 0 && s[0] == ' ') {
            s.erase(s.begin());
        }
        if (s.size() > 0 && s[s.size() - 1] == ' ') {
            s.erase(s.end() - 1);
        }

        length = s.size();
        //整个句子翻转
        int left = 0;
        int right = length - 1;
        while (left < right) {
            swap(s[left], s[right]);
            left++;
            right--;
        }

        //每个单词翻转
        left = 0;
        for (int i = 0; i < length; i++) {
            if (s[i] == ' ') {
                reverse(s.begin() + left, s.begin() + i);
                left = i + 1;
            }
            if (i == length - 1) {
                reverse(s.begin() + left, s.begin() + i + 1);
            }
        }
        return s;
    }
};
```

## 剑指Offer58-II左旋转字符串

**链接**

https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        reverse(s.begin(), s.begin() + n);
        reverse(s.begin() + n, s.end());
        reverse(s.begin(), s.end());
        return s;
    }
};
```


## 28实现strStr()

**链接**

https://leetcode.cn/problems/implement-strstr/

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        //KMP算法
        int len_hay = haystack.size();
        int len_needle = needle.size();
        if (len_needle == 0) { //特判
            return 0;
        }

        //求解needle字符串的next数组
        vector<int> next(len_needle);
        //i指向后缀起始位置，j指向前缀起始位置
        for (int i = 1, j = 0; i < len_needle; i++) {
            //只要当前位置不相等，j向前移动
            while (j > 0 && needle[i] != needle[j]) {
                j = next[j - 1];
            }
            //如果前后缀当前字符相等，则j向后移动一位
            if (needle[i] == needle[j]) {
                j++;
            }
            //next数组赋值，赋值结束后i向后移动一位
            next[i] = j;
        }

        //查找,开始时i指向hay_stack起始，j指向needle起始
        //next里面存的是0到当前位置相同前缀和后缀的最大值
        for (int i = 0, j = 0; i < len_hay; i++) {
            //如果不同，j移动到next数组保存的位置，省去一些重复匹配过程
            //注意这里while循环必须在下个if之前，防止needle[j]溢出导致的while条件为真
            while (j > 0 && haystack[i] != needle[j]) { 
                j = next[j - 1];
            }
            //如果相同，i和j同时向前移动
            if (haystack[i] == needle[j]) { 
                j++;
            }
            //匹配结束时，i在haystack的匹配末尾
            if (j == len_needle) { 
                return i - len_needle + 1;
            }
        }
        return -1; //无匹配结果
    }
};
```

## 459重复的子字符串

**链接**

https://leetcode.cn/problems/repeated-substring-pattern/

**思路**

见代码随想录

**代码**

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        //KMP算法，构建next数组
        int length = s.size();
        vector<int> next(length);
        for (int i = 1, j = 0; i < length; i++) {
            while (j > 0 && s[i] != s[j]) {
                j = next[j - 1];
            }
            if (s[i] == s[j]) {
                j++;
            }
            next[i] = j;
        }

        //根据next数组判断是否满足条件
        //length - next[length - 1]表示重复子串的起始位置
        //如果能被length整除，表示刚好可以由这个子串组成
        if (next[length - 1] != 0 && (length % (length - next[length - 1])) == 0) {
            return true;
        }
        return false;
    }
};
```

## 总结与思考

- 字符串string的使用和vector非常像，函数接口也基本一致
- [151. 颠倒字符串中的单词](#151.-颠倒字符串中的单词) 一题，代码随想录给出的解法非常有意思，不停翻转局部字符即可，想法简单，而且是线性时间复杂度
- 使用KMP算法可以解决两类经典问题：
    - 在一个字符串中找另一个字符串: [28. 实现 strStr()](#28.-实现-strstr())
    - 找重复子字符串: [459. 重复的子字符串](#459.-重复的子字符串)
- KMP算法有点难，代码随想录和leetcode官方给出的解析可以同时看，有助于理解。题: [28. 实现 strStr()](#28.-实现-strstr())
- KMP算法的关键是构建next数组，构建时注意i, j和next[i]分别表示的是什么。题: [28. 实现 strStr()](#28.-实现-strstr())
- 还要注意构建过程中while循环和if判断的顺序不能变，防止索引````j越界导致的错误。具体见题: [28. 实现 strStr()](#28.-实现-strstr())
- [459. 重复的子字符串](#459.-重复的子字符串) 一题中，代码随想录中给出的判断条件非常有意思，比现有解法要更容易理解


## 参考资料

- https://programmercarl.com/
- https://www.bilibili.com/video/BV1PD4y1o7nd/
