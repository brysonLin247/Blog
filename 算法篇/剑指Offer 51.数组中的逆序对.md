## 数组中的逆序对

> 剑指Offer 51.数组中对逆序对
>
> 难度：困难
>
> 题目：https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/

在数组的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

示例1：

```
输入: [7,5,6,4]
输出: 5
```

限制：0 <= 数组长度 <= 50000

## 题解

### 法一 暴力法

两层for循环去检查是否为逆序对。

```javascript
var reversePairs = function (nums) {
  let sum = 0;
  const len = nums.length;

  for (let i = 0; i < len; i++) {
    for (let j = i + 1; j < len; j++) {
      nums[i] > nums[j] && sum++;
    }
  }
  return sum;
}
```

- 时间复杂度：O($N^2$)
- 空间复杂度：O($1$)

### 法二 归并排序

能够看到非常明显的阶段排序结果的算法就是归并排序，采用分而治之的思想：

- 将数组不断的进行一拆二，拆到每个数组中只有一个元素为止
- 对每个数组两两进行合并，其中每个数组内都是有序的

对于本题：

先设置一个sum来记录逆序对个数，采用归并排序的方式，在合并时，设置i和j分别对应两个合并数组的左部分left和右部分right，当`left[i] > right[j]`时说明当前[i, left.length]的值均大于right[j]，因此均可以作为逆序对，故`sum += left.length - i`。

```javascript
var reversePairs = function (nums) {
  let sum = 0;
  mergeSort(nums);
  return sum;

  function mergeSort(nums) {
    if (nums.length < 2) return nums;
    const mid = Math.floor(nums.length / 2);
    let left = nums.slice(0, mid);
    let right = nums.slice(mid);
    return merge(mergeSort(left), mergeSort(right));
  }

  function merge(left, right) {
    let res = [];
    let lLen = left.length;
    let rLen = right.length;
    let Len = lLen + rLen;

    for (let index = 0, i = 0, j = 0; index < Len; index++) {
      if (i >= lLen) res[index] = right[j++]; // i遍历完了
      else if (j >= rLen) res[index] = left[i++]; // j遍历完了
      else if (left[i] <= right[j]) res[index] = left[i++]; 
      else {
        res[index] = right[j++];
        sum += lLen - i;
      }
    }
    return res;
  }
} 
```

- 时间复杂度：O($NlogN$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

