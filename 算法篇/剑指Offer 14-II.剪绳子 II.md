## 剪绳子 II

> 剑指Offer 14 - II. 剪绳子 II

给你一根长度为n的绳子，请把绳子剪成整数长度的m段（m、n都是整数，n > 1并且m > 1），每段绳子的长度记为k[0]，k[1]...k[m-1]。请问k[0] * k[1] * ... * k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回1。

示例1：

```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
```

示例2：

```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
```

提示：2 <= n <= 1000

## 题解

### 法一 BigInt + 动态规划

与上题《剪绳子》不同的是，本题需要涉及“大数越界的求余问题”。因为Math.max不能求BigInt类型的最值，所以我们可以自己写个函数判断最值。

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var cuttingRope = function (n) {
    let dp = new Array(n+1).fill(BigInt(1));
    for (let i = 3; i <= n; i++) {
        for (let j = 1; j < i; ++j) {
            dp[i] = max(dp[i], dp[j] * BigInt((i - j)), BigInt(j * (i - j)));
        }
    }
    return dp[n] % (1000000007n);
};

const max = (...args) => args.reduce((prev, curr) => prev > curr ? prev : curr)
```

### 法二 

- 一般求幂计算可以用Math.pow()
- 如果极大值的情况，那么用for循环替换，并对其进行取模运算，这样就不会造成值的溢出

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var cuttingRope = function(n){
  if(n===2) return 1;
  if(n===3) return 2;
  // a的含义：n能拆成的3的个数
  const a = Math.floor(n/3);
  const b = n % 3;
  let max = 1;
  // n是3的倍数
  if(b===0){
    // Math.pow(3,a);
    for(let i = 0;i < a;i++){
      max = 3 * max % (1e9+7);
    }
    return max;
  } 
  // n是3k+1
  if(b===1) {
    max = 4;
    for(let i = 0;i < a-1;i++){
      max = 3 * max % (1e9+7);
    }
    return max;
  }
  max = 2;
  for(let i = 0;i < a;i++){
    max = 3 * max % (1e9+7);
  }
  return max;
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

