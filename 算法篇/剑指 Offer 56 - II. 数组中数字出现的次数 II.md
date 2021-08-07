## 数组中数字出现的次数 II

> 剑指 Offer 56 - II. 数组中数字出现的次数 II
>
> 难度：中等
>
> 题目：https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/

在一个数组nums中除一个数字只出现了一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

示例1：

```
输入：nums = [3,4,3,3]
输出：4
```

示例2：

```
输入：nums = [9,1,7,9,7,9,7]
输出：1
```

## 题解

### 法一 哈希表

遍历数组，统计每个数字出现次数，最后再遍历一遍哈希表，返回出现次数为1的值。

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function (nums) {
  const map = new Map();
  // 统计每个数字出现次数
  for (let num of nums) {
    if (map.has(num)) {
      map.set(num, map.get(num) + 1);
    } else {
      map.set(num, 1);
    }
  }
	// 遍历哈希表，找出出现次数为1的值
  for (let [_, value] of map.entries()) {
    if (value === 1) return value;
  }
};

//console.log(singleNumber([9, 1, 7, 9, 7, 9, 7]))
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

### 法二 数学公式法

若a、b、c中，a、b出现3次，c出现1次，则可以利用`3*(a+b+c)-(3*a+3*b+c) = 2c`算出c的值。

将数组里面的所有值进行记录并进行去重，可以用集合Set。

```javascript
var singleNumber = function (nums) {
  // 1.数组去重
  let set = new Set(nums);
  let sum1 = 0,
    sum2 = 0
  
  // 设置sum1，为例子中的(a+b+c)
  for (let num of set) {
    sum1 += num;
  }
  // 设置sum2，为例子中的(3*a+3*b+c)
  for (let num of nums) {
    sum2 += num;
  }
	// 返回结果
  return Math.floor((3 * sum1 - sum2) / 2)
}
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

### 法三 位运算

这里我们无法像上道题那样采用异或的方法，但是我们可以沿用位运算的思路。

如果一个数字出现了三次，说明二进制表示的每一位也出现了三次。如果把所有出现三次的数字的二进制表示的每一位分别加起来，那么每一位的和都能被3整除。如果某一位的和能被3整除，那么那个只出现一次的数字二进制表示中对应的那一位是0；否则就是1.

上诉思路同样适用于数组中一个数字出现一次，其他数字出现奇数次问题（如果是偶数次，直接用异或）。

步骤：

- 设置初始值
- 让数组中每个数和掩码进行与运算，如果结果不为0，则count加1
- 如果count能整除3，说明不是出现一次的数字

```javascript
var singleNumber = function (nums) {
  let res = 0;
  // 二进制最高位32位
  for (let bit = 0; bit < 32; bit++) {
    let mask = 1 << bit;
    let count = 0;
    for (let num of nums) {
      if (num & mask) count++;
    }
    if (count % 3) {
      res = res | mask;
    }
  }
  return res;
}
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

