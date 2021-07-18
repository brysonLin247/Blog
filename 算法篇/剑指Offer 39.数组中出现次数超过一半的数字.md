## 数组中出现次数超过一半的数字

> 剑指Offer 39.数组中出现次数超过一半的数字
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例1：

```
输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
```

限制：1 <= 数组长度 <= 50000

## 题解

由本题可知，数字出现的次数超过数组长度的一半（这里简称“众数”）可得出关键三个解法：

- **哈希表法**：遍历数组nums，用Map统计出数字的数量，即可找出众数，时间空间复杂度均为O(N)。
- **数组排序法**：将数组nums排序，数组**中点**一定为众数。
- **摩尔投票法**：核心理念为**票数正负抵消**，时间复杂度为O(N)，空间复杂度为O(1)。

### 法一 哈希表法

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function(nums) {
	var map = new Map();
  for(let i = 0;i < nums.length;i++){
    // 将数组元素和出现次数都存进map中
    if(map[nums[i]] === undefined){
    	map[nums[i]] = 1;   
    }else{
      map[nums[i]]++;
    }
  }
  // 遍历map中对应个数超过nums长度一半都元素
  for(let item in map){
    if(map[item] >= Math.ceil(nums.length/2)){
      return item;
    }
  }
};
```

###  法二 数组排序法

```javascript
var majorityElement = function(nums) {
  let num = nums.sort();
  return num[Math.floor(num.length/2)];// 四舍，避免只输入一个数字时，五入是undefined
}
```

### 法三 摩尔投票法

原理类似于同归于尽，众数与非众数间一一抵消，最终留下的将会是众数（votes > 0）。

```javascript
var majorityElement = function(nums) {
  let x = 0, votes = 0; // x为众数，votes为合计数
  for(let i = 0;i < nums.length;i++){
    if(!votes){ // 若votes为0，则将众数设为当前下标值
      x = nums[i];
      votes++;
    }else{ // 下标值是否为众数，是则加1，否则减1
      votes += nums[i] === x ? 1 : -1;
    }
  }
  return x;
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

