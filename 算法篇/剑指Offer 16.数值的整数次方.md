## 数值的整数次方

> 剑指Offer 16.数值的整数次方

实现`pow(x,n)`，即计算x的n次幂函数（即，$x^n$）。不得使用库函数，同时不需要考虑大数问题。

示例1：

```
输入：x = 2.00000, n = 10
输出：1024.00000
```

示例2：

```
输入：x = 2.10000, n = 3
输出：9.26100
```

示例3：

```
输入：x = 2.00000, n = -2
输出：0.25000
解释：2^(-2) = (1/2)^2 = 1/4 = 0.25
```

## 题解

### 法一 递归

使用递归的方法：

- 当n=0时，任何x都返回1
- 当n=1时，返回x
- 当n=-1时，返回1/x

对于其他n值：

- 当n为偶数时，myPow(x,n) = myPow(x,n/2)*myPow(x,n/2)
- 当n为奇数时，myPow(x,n) = myPow(x,(n-1)/2)*myPow(x,(n-1)/2) * x

注意：递归时先用一个变量取得myPow(x,n/2)的值再平方，可以降低时间复杂度（减少递归调用的次数）

```javascript
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
 */
var myPow = function(x,n){
  if(n===0) return 1;
  if(n===1) return x;
  if(n===-1) return 1/x;
  if(n%2===0){
    let a = myPow(x,n/2);
    return a*a;
  }
  else{
    let b = myPow(x,(n-1)/2);
    return b*b*x;
  }
}
```

### 法二 快速幂

假设base=3，exponent=5。那么5的二进制是：101。所以，3的5次方可以写成下图：

![img](https://pic.leetcode-cn.com/07b61523ab54c8acd38db44abee96017e29cddfa387488bbaabb3313fe8a842e.jpg)

对base进行自乘，导致base的指数每次都扩大2倍。与exponent的二进制相对应。

以上图为例，整个算法的流程如下：

- 结果值result初始为1
- base初始为3，此时exponent的二进制最右位为1，更新结果为：base * result
- exponent右移一位。base进行累乘，base更新为3的2次方。由于exponent的二进制最右位为0，不更新结果
- exponent右移一位。base进行累乘，base更新为3的4次方。此时exponent的二进制最右位为1，更新结果为：base * result
- 结束

```javascript
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
 */
var myPow = function(x, n) {
    let reusult = 1.0;
    //如果负数，2^-2可以编程 （1/2）^2
    if(n<0){
        //js中默认不是整除
        x = 1/x;
        n = -n;
    }
    while(n>0){
        if(n&1){ // 如果n最右位是1，将当前x累乘到result
            reusult*=x;
        }
        x*=x; // x自乘法
        //>>是有符号数的移位，>>>是无符号数的移位
        //下面第一种是错误的，剩下两个都是正确的
        // n = n>>1
        // n = Math.floor(n/2)
        n = n>>>1
    }
    return reusult;
};
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

