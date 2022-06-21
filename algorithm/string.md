---
layout: post
---


## 前言


## 题目列表

| 题目 | 链接 | 
|:-----|:------|
| 344反转字符串 | [https://leetcode.cn/problems/reverse-string/](https://leetcode.cn/problems/reverse-string/) |
| 541反转字符串II | [https://leetcode.cn/problems/reverse-string-ii/](https://leetcode.cn/problems/reverse-string-ii/) |
| 剑指Offer05替换空格 | [https://leetcode.cn/problems/ti-huan-kong-ge-lcof/](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/) |
| 151颠倒字符串中的单词 | [https://leetcode.cn/problems/reverse-words-in-a-string/](https://leetcode.cn/problems/reverse-words-in-a-string/) |
| 剑指Offer58-II左旋转字符串 | [https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/) |
| 28实现strStr() | [https://leetcode.cn/problems/implement-strstr/](https://leetcode.cn/problems/implement-strstr/) |
| 459重复的子字符串 | [https://leetcode.cn/problems/repeated-substring-pattern/](https://leetcode.cn/problems/repeated-substring-pattern/) |


## 总结与思考

- 字符串string的使用和vector非常像，函数接口也基本一致
- [151. 颠倒字符串中的单词]一题，代码随想录给出的解法非常有意思，不停翻转局部字符即可，想法简单，而且是线性时间复杂度
- 使用KMP算法可以解决两类经典问题：
    - 在一个字符串中找另一个字符串: [28. 实现 strStr()]
    - 找重复子字符串: [459. 重复的子字符串]
- KMP算法有点难，代码随想录和leetcode官方给出的解析可以同时看，有助于理解。题: [28. 实现 strStr()]
- KMP算法的关键是构建next数组，构建时注意i, j和next[i]分别表示的是什么。题: [28. 实现 strStr()]
- 还要注意构建过程中while循环和if判断的顺序不能变，防止索引````j越界导致的错误。具体见题: [28. 实现 strStr()]
- [459. 重复的子字符串]一题中，代码随想录中给出的判断条件非常有意思，比现有解法要更容易理解


## 参考资料

- [代码随想录](https://programmercarl.com/)
- [KMP算法解读](https://www.bilibili.com/video/BV1PD4y1o7nd/)
