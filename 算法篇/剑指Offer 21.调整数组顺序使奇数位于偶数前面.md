## 调整数组顺序使奇数位于偶数前面

> 剑指Offer 21.调整数组顺序使奇数位于偶数前面
>
> 难度：简单

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

示例：

```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

提示：

1. 0 <= nums.length <= 50000
2. 1 <= nums[ i ] <= 10000

## 题解

### 法一 sort函数

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var exchange = function(nums) {
	return nums.sort((a,b)=>b%2-a%2)
};
```

### 法二 暴力法

需要做2次循环：

- 第一次循环找到偶数和奇数，并存放到各自数组。
- 第二次循环将两个数组连接

```javascript
var exchange = function(nums){
  const arr = [];
  const brr = [];
  nums.forEach(item => {
    item % 2 ? arr.push(item) : brr.push(item);
  });
  return arr.concat(brr);
}
```

时间复杂度、空间复杂度均为O(n)

### 法三 双指针法

将指针分别指向数组头部的指针i，与指向数组尾部的指针j。过程如下：

- i向右移动，直到遇到偶数；j向左移动，直到遇到奇数
- 检查i是否小于j，若小于则交换i和j的元素，回到上一步骤继续移动；否则结束循环

```javascript
var exchange = function(nums){
  const length = nums.length;
  if(!length){
    return [];
  }
  let i = 0,j = length - 1;
  while(i < j){
    while(i < length && nums[i] % 2 === 1) i++;
    while(j >= 0 && nums[j] % 2 === 0) j--;
    if(i < j){
      [nums[i],nums[j]] = [nums[j],nums[i]];
      i++;
      j--;
    }
  }
  return nums;
}
```

时间复杂度O(n)、空间复杂度O(1)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～