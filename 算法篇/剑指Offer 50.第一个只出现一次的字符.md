## 第一个只出现一次的字符

> 剑指Offer 50.第一个只出现一次的字符
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/

在字符串s中找出第一个只出现一次的字符。如果没有，返回一个单空格。s只包含小写字母。

示例：

```
s = "abaccdeff"
返回 "b"

s = "" 
返回 " "
```

限制： 0 <= s的长度 <= 50000

## 题解

### 法一 使用api函数

使用indexOf和lastIndexOf巧妙解决问题。

```javascript
/**
 * @param {string} s
 * @return {character}
 */
var firstUniqChar = function (s) {
  for (let x of s) {
    if (s.indexOf(x) === s.lastIndexOf(x)) {
      return x;
    }
  };
  return ' ';
};
```

### 法二 哈希表

步骤：

- 对字符串进行循环，如果字符第一次出现，则在哈希表中对其置1，如果不是第一次出现的话则进行累加。
- 遍历哈希表，返回第一个出现次数为1的字符。

```javascript
var firstUniqChar = function (s) {
  if(!s) return ' ';
  let dic = new Map();
  for (let c of s) {
    if (dic.has(c)) {
      dic.set(c, dic.get(c) + 1)
    } else {
      dic.set(c, 1);
    }
  }

  for (let x of dic.keys()) {
    if (dic.get(x) === 1) {
      return x;
    }
  }
  return ' ';
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

