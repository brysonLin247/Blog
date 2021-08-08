## 和为s的两个数字

> 剑指Offer 57.和为s的两个数字
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/

输入一个递增排序的数组和为一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

示例1：

```
输入：nums = [2,7,11,15], target = 9
输出：[2,7] 或者 [7,2]
```

示例2：

```
输入：nums = [10,26,30,31,47,60], target = 40
输出：[10,30] 或者 [30,10]
```

限制：

- 1 <= nums.length <= 10^5
- 1 <= nums[i] <= 10^6

## 题解

### 法一 双指针

注意到数组是一个递增排序的数组，我们可以使用双指针来做，思路如下：

- 设置i指针指向数组首位，j指向数组末尾
- 若`nums[i] + num[j] > target`，指针j需要左移一位，反之，指针i需要右移一位。
- 若`nums[i] + num[j] = target`，返回结果[i, j]。

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function (nums, target) {
  let i = 0,
    j = nums.length - 1;
  while (i < j) {
    if (nums[i] + nums[j] > target) {
      j--;
    } else if (nums[i] + nums[j] < target) {
      i++;
    } else {
      return [nums[i], nums[j]];
    }
  }
  return null;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

### 法二 哈希表

将nums中出现的数字存入哈希表，然后在遍历循环的时候，查看有无`target - i`的值存在，存在的话则返回结果。

```javascript
var twoSum = function (nums, target) {
  let map = {};
  nums.forEach((item) => {
    map[item] = item;
  });
  for (let i of nums) {
    if (map[target - i]) {
      return [target - map[i], i];
    }
  }
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～
