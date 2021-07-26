## 把数组排成最小的数

> 剑指Offer 45.把数组排成最小的数
>
> 难度：中等
>
> 题目：https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

示例1：

```
输入: [10,2]
输出: "102"
```

示例2：

```
输入: [3,30,34,5,9]
输出: "3033459"
```

提示：0 < nums.length <= 100

说明：

- 输出结果可能非常大，所以你需要返回一个字符串而不是整数
- 拼接起来的数字可能会有前导0，最后结果不需要去掉前导0

## 题解

例如[10, 2]，设a = 10，b = 2，则a+b = “102”，b+a=“210”，要拼接成最小的数，则：

- 如果`a + b < b + a`，则ba比ab大，a应该在b的左边

- 如果`a + b < b + a`，则ab比ba大，a应该在b的右边

因此a在b的左边，可以看出本题可以使用排序的方法解决。

### 法一 内置函数

```javascript
/**
 * @param {number[]} nums
 * @return {string}
 * 数组内排序后，再将数组转字符串。
 */
var minNumber = function(nums) {
	return nums.sort((a, b) => 
    ('' + a + b) - ('' + b + a)
  ).join(''); 
};
```

- 时间复杂度：O($NlogN$)，sort函数使用了快排
- 空间复杂度：O($1$)

### 法二 快排

避免被面试官打死，还是手写一下快排叭～

```javascript
function quickSort(array, start, end) {
  if (end - start < 1) return;
  const target = array[start];
  let l = start;
  let r = end;
  while (l < r) {
    while (l < r && array[r] + target >= target + array[r]) {
      r--;
    }
    array[l] = array[r];
    while (l < r && array[l] + target < target + array[l]) {
      l++;
    }
    array[r] = array[l];
  }
  array[l] = target;
  quickSort(array, start, l - 1);
  quickSort(array, l + 1, end);
  return array;
}
var minNumber = function (nums) {
  if (nums.length < 2) return String(nums);
  // 将nums转为字符串数组
  return quickSort(nums.map(String), 0, nums.length - 1).join("");
}
```

- 时间复杂度：O($NlogN$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～



