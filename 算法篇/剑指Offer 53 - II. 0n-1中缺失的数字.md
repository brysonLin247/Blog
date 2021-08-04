## 0~n-1中缺失的数字

> 剑指Offer 53 - II. 0~n-1中缺失的数字
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

示例1：

```
输入: [0,1,3]
输出: 2
```

示例2：

```
输入: [0,1,2,3,4,5,6,7,9]
输出: 8
```

限制：1 <= 数组长度 <= 10000

## 题解

遇到排序数组中的搜索问题，首先要想到「二分查找」解决！

### 二分查找

因为范围是从0开始的，所以我们的下标刚好可以当作元素的值。

- 设left指向0，right指向末尾元素，计算中间值mid
- 循环条件为`left <= right`，mid值判断：
  - `mid > nums[mid]`，由题意知，不存在这种情况。
  - `mid < nums[mid]`，说明[left, mid]内缺失元素，要对该区域继续二分查找，因此right要更新为mid - 1。
  - `mid = nums[mid]`，说明[left, mid]内部缺失元素，要对 [mid, right]继续二分查找，因此left要更新为mid + 1。
- 返回left的下标即为缺失的值。

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var missingNumber = function (nums) {
  let left = 0,
    right = nums.length - 1;
  while (left <= right) {
    let mid = Math.floor((left + right) / 2);
    if (mid < nums[mid]) {
      right = mid - 1;
    } else if (mid === nums[mid]) {
      left = mid + 1;
    }
  }
  return left;
};
```

- 时间复杂度：O($logN$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

