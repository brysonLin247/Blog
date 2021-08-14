## 扑克牌的顺子

> 剑指Offer 61. 扑克牌中的顺子
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/

从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王都为0，可以看成任意数字。A不能视为14。

示例1：

```
输入: [1,2,3,4,5]
输出: True
```

示例2：

```
输入: [0,0,1,2,5]
输出: True
```

限制：数组长度为5，数组的数取值为[0,13]。

## 题解

一开始看到示例2很疑惑，为啥`[0,0,1,2,5]`是一个顺子，其实就是说0是一张万能牌，用0可以充当任何一张牌，就可以将其看成`[1,2,3,4,5]`啦。

判断5张牌是顺子的条件如下：

- 除了大小王外，其他所有牌无重复
- 5张牌中需满足「最大的牌 - 最小的牌（大小王除外） < 5」

### 法一 集合 + 遍历

- 使用Set去重
- 遍历的时候获取最大牌max和最小牌min。

```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var isStraight = function (nums) {
  const set = new Set();
  let max = 0,
    min = 14;
  for (let num of nums) {
    if (num === 0) continue;
    max = Math.max(max, num);
    min = Math.min(min, num);
    if (set.has(num)) return false;
    set.add(num);
  }
  return max - min < 5;
};
```

- 时间复杂度：O($N$)，本题中N为5
- 空间复杂度：O($N$)

### 法二 排序 + 遍历

- 先对数组排序
- 判断nums[i]是否与nums[i+1]相等（判断重复），相等则直接返回false
- 排序后，末尾元素nums[4]为最大牌，元素nums[joker]为最小牌，其中joker为大小王的数量

```javascript
var isStraight = function (nums) {
  let joker = 0; // 用于判断大小王的数量，从而作为下标
  nums.sort((a, b) => a - b); // 记得加上sort()内的函数
  // for循环只需要判断前4位即可
  for (let i = 0; i < 4; i++) {
    if (nums[i] === 0) joker++;
    else if (nums[i] === nums[i + 1]) return false;
  }
  return nums[4] - nums[joker] < 5;
};
```

- 时间复杂度：O($NlogN$)，sort使用的是快排需要O($NlogN$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

