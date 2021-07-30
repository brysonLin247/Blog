## 最长不含重复字符的子字符串

> 剑指Offer 48.最长不含重复字符的子字符串
>
> 难度：中等
>
> 题目：https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

示例1：

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

示例2：

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

示例3：

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

提示：s.length <= 40000

## 题解

  一个长度为N的字符串共有$1+2+3+...+N = 1/2*N*(1+N)$个子字符串，而判断子字符串是否有重复的复杂度为O($N$)，因此暴力法的话复杂度为O($N^3$)，显然这种方法AC不了。

首先，我们可以考虑使用动态规划来降低时间复杂度。

步骤：

- 设dp[j]为以j结尾的“最长不重复子字符串”的长度。

- 固定右边界j，设字符s[j]左边距离最近且相等的字符为s[i]，以字符s[j - 1]结尾的子字符串sub[j - 1] ，其长度为dp[j - 1]，注意sub[j - 1]中字符不重复。在j的左侧寻找一个重复的字符s[i]，需要分两种情况：

  - **索引i在字符串sub[j - 1]区间外**，也就是sub[j-1]左边界的更左侧，则dp[j-1] < j - i，，由于sub[j - 1]中字符不重复，且当前最长，因此子字符串sub[j - 1]在后面接上s[j]即可，其长度dp[j] = dp[j - 1]。
  - **索引i在字符串sub[j - 1]区间内**，则dp[j - 1]  ≥ j - i，则sub[j]的左边界只能是i了，其长度dp[j] = j - i。

   这里举个例子，如`[abcdbaa]`，索引从0开始，当j=4时，sub[4] = “cbd”，长度dp[4] = 3；当j = 5时，s[0] = s[5]且s[0]在sub[4]之外，因此长度dp[5] = dp[4] + 1；当j = 6时，s[5]=s[6]且s[5]在sub[5]内，则dp[6] = j - i = 1。

  因此，公式为：
  $$
  dp|j| = \begin{cases} dp[j-1] + 1  ，dp[j-1]<j-i\\ j - i\quad\quad\quad，dp[j-1]≥ j-i\end{cases}
  $$

- 返回dp中最大值即为“最长不重复子字符串”的长度。这里可以借助一个变量tmp存粗dp[j]，每轮循环的时候更新最大值即可，可以省创建dp数组O($N$)的空间。

### 法一 动态规划 + 哈希表

使用哈希表来统计**各字符最后一次出现的索引位置**，遍历到s[j]时，可通过哈希表dic[s[j]]获取最佳的相同字符的索引i。

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function (s) {
  let dic = new Map();
  let res = 0,
    tmp = 0;
  for (let j = 0; j < s.length; j++) {
    let i = dic.has(s[j]) ? dic.get(s[j]) : -1;
    dic.set(s[j], j);
    tmp = tmp < j - i ? tmp + 1 : j - i;
    res = Math.max(res, tmp);
  }
  return res;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

### 法二 动态规划 + 遍历

遍历到s[j]时，初始化索引i = j - 1，向左遍历搜索第一个满足s[i] = s[j]字符即可。

```javascript
var lengthOfLongestSubstring = function (s) {
  let res = 0, tmp = 0;
  for(let j = 0; j < s.length; j++){
    let i = j - 1;
    while(i >= 0 && s[i] !== s[j]) i--;
    tmp = tmp < j - i ? tmp + 1 : j - i;
    res = Math.max(res, tmp);
  }
  return res;
};
```

- 时间复杂度：O($N^2$)
- 空间复杂度：O($1$)

### 法三 滑动窗口

使用哈希表dic统计s[j]**最后一次出现的索引**，步骤如下：

- 更新左边界i，根据上轮做边界i和dic[s[j]]，每次更新左边界i都要保证[i + 1, j]内无重复字符且最大：「$i=max(dic[s[j]],i)$」

- 更新结果res，取上轮res和本轮滑动窗口[i +1 , j]的最大值（即j - i的最大值）

```javascript
var lengthOfLongestSubstring = function (s) {
  let dic = new Map();
  let res = 0,
    i = -1;
  for (let j = 0; j < s.length; j++) {
    if (dic.has(s[j])) {
      i = Math.max(dic.get(s[j]), i);
    }
    dic.set(s[j], j);
    res = Math.max(res, j - i);
  }
  return res;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

