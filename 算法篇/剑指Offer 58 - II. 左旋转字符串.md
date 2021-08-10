## 左旋转字符串

> 剑指Offer 58 - II. 左旋转字符串
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串“abcdefg”和数字2，该函数将返回左旋转两位得到的结果“cdefgab”。

示例1：

```
输入: s = "abcdefg", k = 2
输出: "cdefgab"
```

实例2：

```
输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"
```

限制：1 <= k < s.length <= 10000

## 题解

### 法一 字符串拼接

```javascript
/**
 * @param {string} s
 * @param {number} n
 * @return {string}
 */
var reverseLeftWords = function (s, n) {
  let str = s.substring(n) + s.substring(0, n);
  return str;
};
```

- 时间复杂度：O($N$)
- 空间复杂度O($N$)

### 法二 列表遍历拼接

若面试中不允许使用API，则可以使用这种方法。步骤如下：

- 新建数组res
- 将n+1后的值存入res
- 再向res存入首位至n位的字符
- 数组res转字符串

```javascript
var reverseLeftWords = function (s, n) {
  const res = [];
  for (let i = n; i < s.length; i++) {
    res.push(s[i]);
  }
  for (let i = 0; i < n; i++) {
    res.push(s[i]);
  }
  return res.join('');
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

