---
title: leetcode刷题
date: 2020-06-10 14:17:27
tags: leetcode
---

> #### [面试题64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)
>
> 求 `1+2+...+n` ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）

```
var sumNums = function(n) {
   if(n == 1) return 1;
    n += sumNums(n - 1);
    return n;
};
```

> #### [面试题65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)
>
> 写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。
>
>  **示例:**
>
> ```javascript
> 输入: a = 1, b = 1
> 输出: 2
> ```

**提示：**

- `a`, `b` 均可能是负数或 0
- 结果不会溢出 32 位整数

```
var add = function(a, b) {
    if(a == 0) return b;
    if(b == 0) return a;
    return add(( a ^ b ),((a & b) << 1));
};
```

> #### [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)
>
> 判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

```
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    if (x < 0) {
        return false
    }
    x += ''
    let xlen = x.length
    // 利用回文特点，循环长度一半即可（正序倒序相同）
    for (let i = 0; i < ((xlen + 1) / 2) | 0; i++) {
        if (x[i] != x[xlen - i - 1]) {
            return false
        }
    }
    return true;
};
```

> #### [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)
>
> 难度中等363收藏分享切换为英文关注反馈
>
> 根据每日 `气温` 列表，请重新生成一个列表，对应位置的输出是需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 `0` 来代替。
>
> 例如，给定一个列表 `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是 `[1, 1, 4, 2, 1, 1, 0, 0]`。
>
> **提示：**`气温` 列表长度的范围是 `[1, 30000]`。每个气温的值的均为华氏度，都是在 `[30, 100]` 范围内的整数。

单调栈问题

![](/images/单调栈.png)

```javascript
var dailyTemperatures = function(temperatures) {
    if(temperatures.length===0) return []
    let res = new Array(temperatures.length).fill(0)
    let stack = [0]
    for(let i =1;i<temperatures.length;i++){
        while(stack.length>0&&temperatures[i]>temperatures[stack[stack.length - 1]]){
            let index = stack.pop()
            res[index] = i- index   //更新结果数组
        }
        stack.push(i)  //入栈元素小于栈顶元素

    }
    return res
};
var res = dailyTemperatures([73,74,75,71,69,72,76,73])
```

> #### [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)
>
> 给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。
>
> 说明：
>
> 拆分时可以重复使用字典中的单词。
> 你可以假设字典中没有重复的单词。

```javascript
var wordBreak = function(s, wordDict) {
    let dp = new Array(s.length + 1).fill(false);
    let set = new Set(wordDict);

    // 因为dp[i]的i表示长度，因此dp[0]=true
    dp[0] = true;
    // for中的i表示从下标0开始字符串的长度，因此从1开始
    for (let i = 1; i < s.length + 1; i++) {
        // 判断s[j-i]是否满足题目要求
        // 注意此处是j<i不是j<=i
        for (let j = 0; j < i; j++) {
            console.log(s.substring(j, i))
            if (dp[j] && set.has(s.substring(j, i))) {   //dp[j]表示leet那个项是true
                dp[i] = true;
                break;

            }
        }
        console.log("---")
    }
    console.log(dp)
    return dp[s.length];
}

var b = wordBreak("leetcode", wordDict = ["leet", "code"])
console.log(b)
```

