## 连续子数组的最大和

> 剑指Offer 42.连续子数组的最大和
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/

输入一个整形数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O($N$)

示例1：

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

提示：

- 1 <= arr.length <= 10^5
- -100 <= arr[i] <= 100

## 题解

### 法一 动态规划DP

开辟一个用于「记录nums数组中元素下标为[0, i]的连续子数组最大和」的数组dp[i]。

思路：

- 初始值`dp[0] = nums[0]`
- 若`dp[i - 1] > 0`，则`dp[i] = nums[i] + dp[i - 1]`
- 若`dp[i - 1] <= 0`，则`dp[i] = nums[i]`

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
  const dp = [];
  dp[0] = nums[0]
  let max = dp[0];
  for (let i = 1; i < nums.length; i++) {
    dp[i] = nums[i];
    if (dp[i - 1] > 0) {
      dp[i] += dp[i - 1];
    }
    max = max > dp[i] ? max : dp[i];
  }
  return max;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

### 法二 动态规划空间优化

法一中新开辟了数组，我们可以对其进一步优化使之在原数组上操作。

```javascript
var maxSubArray = function(nums) {
  let max = nums[0];
  for(let i = 1; i < nums.length; i++){
    if(nums[i - 1] > 0){
      nums[i] += nums[i - 1];
    }
    max = max > nums[i] ? max : nums[i];
  }
  return max;
}
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

### 法三 使用reduce函数

`arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])`

callback函数包含四个参数：

- **accumulator**：累计器累计回调的返回值; 它是上一次调用回调时返回的累积值，或`initialValue`。
- **currentValue**：数组中正在处理的元素。
- **index**（可选）：数组中正在处理的当前元素的索引。如果提供了`initialValue`，则起始索引号为0，否则从1开始。
- **array**（可选）：调用**reduce()**的数组。
- **initialValue**（可选）：作为第一次调用 `callback`函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。

返回值为函数累计处理的结果。

```javascript
// 借用了动态规划的思想
var maxSubArray = function(nums) {
	let max = -Infinity;
  nums.reduce((total, cur, i) => {
    if(total > 0){
      total += cur;
    }else{
      total = cur;
    }
    max = max > total ? max : total;
    return total;
  },0) // total初始为0
  return max;
};
```

### 法四 贪心法

```javascript
var maxSubArray = function(nums) {
  let max = nums[0];
  let cur = nums[0];
  for(let i = 1; i < nums.length; i++){
    cur = Math.max(nums[i], cur + nums[i]);
    max = Math.max(max, cur);
  }
  return max;
}
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

