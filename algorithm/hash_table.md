---
layout: post 
---

## 前言

## 题目列表

| 题目 | 链接 | 
|:-----|:------|
| 242有效的字母异位词 | [https://leetcode.cn/problems/valid-anagram/](https://leetcode.cn/problems/valid-anagram/) | 
| 349两个数组的交集 | [https://leetcode.cn/problems/intersection-of-two-arrays/](https://leetcode.cn/problems/intersection-of-two-arrays/) |
| 202快乐数 | [https://leetcode.cn/problems/happy-number/](https://leetcode.cn/problems/happy-number/) |
| 383赎金信 | [https://leetcode.cn/problems/ransom-note/](https://leetcode.cn/problems/ransom-note/) |
| 1两数之和 | [https://leetcode.cn/problems/two-sum/](https://leetcode.cn/problems/two-sum/) |
| 454四数相加II | [https://leetcode.cn/problems/4sum-ii/](https://leetcode.cn/problems/4sum-ii/) |
| 15三数之和 | [https://leetcode.cn/problems/3sum/](https://leetcode.cn/problems/3sum/) |
| 18四数之和 | [https://leetcode.cn/problems/4sum/](https://leetcode.cn/problems/4sum/) |


## 总结与思考

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

## 参考资料

- [代码随想录](https://programmercarl.com/)
