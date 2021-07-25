## 1~n整数中1出现的次数

> 剑指Offer 43.1～n整数中1出现的次数
>
> 难度：困难
>
> 题目：https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/

输入一个整数n，求1～n这n个整数的十进制表示中1出现的个数。

例如，输入12，1～12这些整数中包含1的数字有1、10、11和12，1一共出现了5次。

示例1：

```
输入：n = 12
输出：5
```

示例2：

```
输入：n = 13
输出：6
```

限制：1 <= n < 2^31

## 题解

### 归纳法

这道题纯用归纳解决

从个数到最高位遍历，将n看成「high、cur、low」这三个部分，其中cur为当前位数，high为cur之前的值，low为cur之后的值，根据当前位数，设digit为1、10、100......通过数学归纳法推理出：

- 当`cur = 0`，执行`res += high * digit`
- 当`cur = 1`，执行`res += high * digit + low + 1`
- 当`cur > 1`，执行`res += high * digit + digit`

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var countDigitOne = function (n) {
  let digit = 1,
    res = 0;
  let high = Math.floor(n / 10),
    cur = n % 10,
    low = 0;
  while (high !== 0 || cur !== 0) {
    if (cur === 0) res += high * digit;
    else if (cur === 1) res += high * digit + low + 1;
    else res += high * digit + digit;
    low += cur * digit;
    cur = high % 10;
    high = Math.floor(high / 10);
    digit *= 10;
  }
  return res;
};
```

- 时间复杂度：O($logN$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

