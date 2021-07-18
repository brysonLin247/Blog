## 剑指Offer 03.数组中重复的数字

找出数组中重复的数字。

在一个长度为n的数组nums里的所有数字都在0～n-1的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3
```

限制：

2 <= n <= 100000

## 题解

### 法一 哈希表

空间复杂度O(n) 时间复杂度O(n)

通过题意，我们可以使用哈希表来实现，分析如下：

1. 遍历数组，若当前数字不存在于哈希表，则添加到哈希表即可。
2. 若当前数字哈希表中已存在，则返回结果。

```javascript
var findRepeatNumber = function(nums){
  let map = new Map();
  for(let i of nums){
    if(map.has(i)){
			return i;
    }
    map.set(i,1);
  }
  return null;
}
```

结果：

![image-20210622235525317](/Users/brysonlin/Library/Application Support/typora-user-images/image-20210622235525317.png)

### 法二 原地交换

剑指Offer中的解法，空间复杂度O(1) 时间复杂度O(n)

因为在一个长度为n点数组nums里的所有数字都在0～n-1的范围内，所以数组元素的**索引**和**值**是**一对多**的关系，因此可以建立索引和值的映射，起到字典等价的作用。

![Picture0.png](https://pic.leetcode-cn.com/1618146573-bOieFQ-Picture0.png)

思路：

1. 从头到尾依次扫描，如果当前项和下标不相等，则比较当前项和以当前项对应的下标的项是否相等，不相等则交换位置
2. for循环中有一个while，但是由于每一项最多只会在while中交换两次，所以总体的时间复杂度仍然是O(n)

```javascript
var findRepeatNumber = function(nums) {
    for(let i = 0;i < nums.length;i++){
      let num = nums[i];
      while(num !== i){
        if(nums[i] === nums[num]){
          return num
        }
        nums[i] = nums[num]
        nums[num] = num
      }
    }
  return null
};
```

结果：

![image-20210622235427663](/Users/brysonlin/Library/Application Support/typora-user-images/image-20210622235427663.png)

