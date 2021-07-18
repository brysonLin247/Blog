## 打印从1到最大的n位数

> 剑指Offer 17.打印从1到最大的n位数

输入数字n，按顺序打印出从1到最大的n位十进制数。比如输入3，则打印出1、2、3一直到最大的3位数999.

示例1：

```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```

说明：

- 用返回一个整数列表来代替打印
- n为正整数

## 题解

使用内置函数即可，可以使用快速幂来优化。

```javascript
/**
 * @param {number} n
 * @return {number[]}
 */
var printNumbers = function(n){
  let max = ''
  while(n--) max += '9'
  return new Array(max - '0').fill(0).map((val,index) => index + 1)
}

或

var printNumbers = function(n) {
    let len = Math.pow(10, n)-1
    return Array.from({length: len}, (item, index) => index+1)
};
```



****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～