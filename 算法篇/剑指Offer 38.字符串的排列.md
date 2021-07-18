## 字符串的排列

> 剑指Offer 38.字符串的排列
>
> 难度：中等
>
> leetcode地址：https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/

输入一个字符串，打印出该字符串中字符的所有排列。

你可以任意顺序返回这个字符串数组，但里面不能有重复元素。

示例：

```
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```

限制：1 <= s的长度 <= 8

## 题解

经典的回溯算法，本题求的是全排列+去重，排序使用回溯算法，去重这里直接采用Set，方便省事。经典的回溯算法，本题求的是全排列+去重，排序使用回溯算法，去重这里直接采用Set，方便省事。关于回溯算法理论这里推荐看Carl哥B站教学视频的**带你学透回溯算法！（理论篇）**，能够快速帮我们理解回溯算法的使用场景。

回溯算法（纯暴力的搜索）使用场景：组合、排列、切割、子集、棋盘

```javascript
/**
 * @param {string} s
 * @return {string[]}
 */
var permutation = function(s) {
  const res = new Set();// 用于去重
  const visit = {}
  function dfs(path) {
    if(path.length === s.length) return res.add(path);// 终止条件
    for (let i = 0; i < s.length; i++) {
      if (!visit[i]){
        visit[i] = true;// 标记访问
        dfs(path + s[i]);// 递归
        visit[i] = false;// 回溯，将标记解除
      }
    }
  }
  dfs('');
  return [...res];
};
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

