## 翻转单词顺序

>  剑指Offer 58 - I. 翻转单词顺序
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串“I am a student.”，则输出 “student. a am I”。

示例1：

```
输入: "the sky is blue"
输出: "blue is sky the"
```

示例2：

```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```

示例3：

```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

说明：

- 无空格字符构成一个单词。
- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

## 题解

### 法一 使用API

- trim()删除字符串两端空格
- split(' ')切割空格变为数组
- filter()过滤数组中的空格
- reverse()反转数组
- join(' ')以空格为界限，数组转字符串

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var reverseWords = function (s) {
  let str = s.trim().split(' ').filter(item => item != '').reverse().join(' ');
  return str;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

### 法二 双指针

步骤：

- 去掉首尾空格，从后往前遍历字符串s，记录左右索引边界i和j
- 每确定一个单词的边界，则将单词放入数组res
- 数组res转字符串后返回

```javascript
var reverseWords = function (s) {
  // 1.去掉首尾空格 
  s = s.trim();

  let j = s.length - 1,
    i = j;
  let res = [];

  while (j >= 0) {
    while (i >= 0 && s[i] !== ' ') i--;// 搜索空格
    res.push(s.slice(i + 1, j + 1)); // 添加单词
    while (j >= 0 && s[i] === ' ') i--; // 跳过空格
    j = i; // j指向下一个单词
  }
  return res.join(' '); // 数组转字符串
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

