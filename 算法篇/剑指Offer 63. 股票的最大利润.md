## 股票的最大利润

> 剑指Offer 63. 股票的最大利润
>
> 难度：中等
>
> 题目：https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/

假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

示例1：

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```

示例2：

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

限制：0 <= 数组长度 <= 10^5

## 题解

### 法一 暴力法

两层for循环，直接暴力出最大利润。

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
  let res = 0;
  for (let i = 0; i < prices.length; i++) {
    for (let j = i + 1; j < prices.length; j++) {
      res = Math.max(res, prices[j] - prices[i])
    }
  }
  return res;
};
```

- 时间复杂度：O($N^2$)
- 空间复杂度：O($1$)

### 法二 动态规划

**状态定义**：设`dp[i]`为以`prices[i]`为结尾的子数组的最大利润（即前i日的最大利润）。

**转移方程**：前i日最大利润`dp[i]`为前一天的最大利润`dp[i-1]`和第i天卖出利润`prices[i]-min(prices[0:i])`的最大值，状态方程如下：
$$
dp[i]=max(dp[i-1],prices[i]-min(prices[0:i]))
$$
因为这里的dp[i]只与dp[i-1]有关，这里的dp其实可以用一个变量去取代。

```javascript
var maxProfit = function (prices) {
  let dp = 0,cost = Infinity;
  for(let i = 0; i < prices.length; i++){
    cost = Math.min(cost,prices[i]);
    dp = Math.max(dp,prices[i] - cost);
  }
  return dp;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～