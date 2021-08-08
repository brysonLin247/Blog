## 和为s的连续正数序列

> 剑指Offer 57 - II. 和为s的连续正数序列
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/

输入一个正整数target，输出所有和为target的连续正整数系列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

示例1：

```
输入：target = 9
输出：[[2,3,4],[4,5]]
```

示例2：

```
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```

限制：1 <= target <= 10^5

## 题解

### 滑动窗口

通过示例可以看出，数组最后一个数字，如果`target`是偶数则就是为`target/2`，如果是奇数则就是`target/2`取整加一。

```javascript
/**
 * @param {number} target
 * @return {number[][]}
 */
var findContinuousSequence = function (target) {
  let index = Math.ceil(target / 2);
  // res为结果，temp为结果中的每一个数组，sum用于判断是否等于target
  let res = [],
    temp = [],
    sum = 0;
  for (let i = 1; i <= index; i++) {
    temp.push(i);
    sum += i;
    // 如果sum > target，将temp头部推出，滑动窗口
    while (sum > target) {
      sum -= temp[0];
      temp.shift();
    }
    // 如果在循环过程中，sum等于target，将temp放入res
    if (sum === target) {
      temp.length >= 2 && res.push([...temp]);
    }
  }
  return res;
};

或

// 双指针
var findContinuousSequence = function (target) {
  let l = 1,
    r = 2,
    sum = 3;
  let res = [];
  while (l < r) {
    if (sum === target) {
      let ans = [];
      for (let k = l; k <= r; k++) {
        ans[k - l] = k;
      }
      res.push(ans);
      // 若等于，可以让窗口继续右移，同时缩小左边的
      sum = sum - l;
      l++;
    } else if (sum > target) {
      // 若大于，缩小窗口
      sum = sum - l;
      l++;
    } else {
      // 若小于，扩大窗口
      r++;
      sum = sum + r;
    }
  }
  return res;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～