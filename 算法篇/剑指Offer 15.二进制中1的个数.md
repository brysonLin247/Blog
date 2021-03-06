## 二进制中1的个数

> 剑指Offer 15.二进制中1的个数

编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字为‘1‘的个数（也被称为`汉明重量`）。

示例1:

```
输入：n = 11 (控制台输入 00000000000000000000000000001011)
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
```

示例2:

```
输入：n = 128 (控制台输入 00000000000000000000000010000000)
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
```

示例3:

```
输入：n = 4294967293 (控制台输入 11111111111111111111111111111101，部分语言中 n = -3）
输出：31
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
```

提示：输入必须是长度为32的二进制串

## 题解

### 法一 运用api

```javascript
/**
 * @param {number} n - a positive integer
 * @return {number}
 */
var hammingWeight = function(n) {
    return n.toString(2).split('0').join('').length;
};
```

### 法二 循环检查二进制位

直接循环检查给定整数n的二进制的每一位是否为1。

当检查第i位时，我们可以让n与$2^i$进行与运算，当且仅当n的第i位为1时，运算结果不为0。

1 << 2 即 0001 -> 0010

```javascript
var hammingWeight = function(n) {
	let len = 0;
  for(let i = 0;i < 32;i++){
    if((n & (1 << i))!==0){ // 判断n的第i位是否不为0
      len++;
    }
  }
  return len;
};
```

时间复杂度O(k) （k=32），空间复杂度O(1)

### 法三 位运算优化

观察这个运算：n&(n- 1)，其预算结果恰为把n的二进制中的最低位1变为n之后的结果。

如：6&(6-1)=4，6=$(110)~2$,4=$(100)~2$，运算结果4即为把6的二进制位中的最低位的1变为0之后的结果。

这样我们可以利用这个位运算的性质加速我们的检查过程，在实际代码中，我们不断让当前的n与n-1做与运算，直到n变为0即可。因为每次运算会使得n的最低位的1被翻转，因此运算次数就等于n的二进制中1的个数。

```javascript
var hammingWeight = function(n){
  let ret = 0;
  while(n){
    n &= n-1;
    ret++;
  }
  return ret;
}
```

时间复杂度：O($log n$)。循环次数等于n的二进制位中1的个数，最坏情况下n的二进制位全部为1。我们需要循环$log n$次。

空间复杂度：O(1)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

