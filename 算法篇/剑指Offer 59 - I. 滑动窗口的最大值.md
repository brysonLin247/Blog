## 滑动窗口的最大值

> 剑指Offer 59 - I. 滑动窗口的最大值
>
> 难度：困难
>
> 题目：https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/

给定一个数组nums和滑动窗口的大小k，请找出所有滑动窗口里的最大值。

示例：

```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

提示：你可以假设k总是有效的，在输入数组不为空的情况下，1 <= k <=输入数组的大小。

## 题解

### 法一 暴力法

直接滑动窗口，将每个窗口的数值使用Math.max()获取并放进结果res里面。

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSlidingWindow = function (nums, k) {
  if (k <= 1) return nums;
  const res = [];
  for (let i = 0; i < nums.length - k + 1; i++) {
    res.push(Math.max(...nums.slice(i, i + k)));
  }
  return res;
};
```

- 时间复杂度：O($kN$)
- 空间复杂度：O($N$)

### 法二 单调队列

新增一个**双端队列**用于存储滑动窗口的值的下标，使用**单调队列**可以解决问题，遍历数组时，每轮保证单调队列deque：

1. deque内**仅包含窗口内的元素**，每轮窗口滑动移除了nums[i - 1]，需将deque内的对应元素一起删除。
2. deque内的元素**非严格递减**，每轮窗口滑动添加了元素nums[j + 1]，需将deque内所有比这个心元素小的值删除。

步骤如下：

- 初始化双端队列deque，结果数组res，数组长度n。

- 滑动窗口，明确左边界的范围为[1 - k, n - k]，右边界的范围为[0, n - 1]。
  - 当i > 0且队首deque[0]为被删除元素nums[i - 1]时，队首出队。
  - 删除deque中所有< nums[j]的元素，保持队列单调递减。
  - 将nums[j]添加到尾部。
  - 将窗口最大值（deque[0]）放入结果数组res。
- 返回res。

```javascript
var maxSlidingWindow = function (nums, k) {
  if (nums.length === 0 || k === 0) return [];
  const deque = [];
  const res = [];
  for (let j = 0, i = 1 - k; j < nums.length; i++, j++) {
    // 当i>0时，说明已经有窗口元素已经满了，可以开始滑动并删减元素
    if (i > 0 && deque[0] === nums[i - 1]) {
      deque.shift(); // 窗口元素满了把第一个元素弹出。
    }
    // 要保持队列单调递减，判断队列末尾元素与下一个nums元素大小，队尾小的则要被淘汰
    while (deque && deque[deque.length - 1] < nums[j]) {
      deque.pop(); // 弹出队尾元素
    }
    deque.push(nums[j]); // 将新元素放入队列

    if (i >= 0) {
      res[i] = deque[0]; // 队首元素是每个窗口的最大值，存进res数组
    }
  }
  return res;
};
```

或者，我们可以拆分一下未形成窗口和形成窗口后两个阶段：

```javascript
var maxSlidingWindow = function (nums, k) {
  if (nums.length === 0 || k === 0) return [];
  const deque = [];
  const res = new Array(nums.length - k + 1); // 这样写好像空间复杂度AC出来更低些
  // 未形成窗口
  for (let i = 0; i < k; i++) {
    while (deque && deque[deque.length - 1] < nums[i]) {
      deque.pop(); // 弹出队尾元素
    }
    deque.push(nums[i]);
  }
  res[0] = deque[0];
  // 形成窗口后
  for (let i = k; i < nums.length; i++) {
    if (deque[0] === nums[i - k]) {
      deque.shift();
    }
    while (deque && deque[deque.length - 1] < nums[i]) {
      deque.pop();
    }
    deque.push(nums[i]);
    res[i - k + 1] = deque[0];
  }
  return res;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($k$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～