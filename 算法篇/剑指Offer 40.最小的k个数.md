## 最小的k个数

> 剑指Offer 40.最小的k个数
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/

输入整数数组arr，找出其中最小的k个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4.

示例1：

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

示例2：

```
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

限制：

- 0 <= k <= arr.length <= 10000
- 0 <= arr[i] <= 10000

## 题解

### 法一 直接调api

```javascript
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {number[]}
 */
var getLeastNumbers = function(arr, k){
  // sort使用快排
  return arr.sort((a, b) => a - b).slice(0, k);
}
```

- 时间复杂度：O($NlogN$)
- 空间复杂度：O($logN$)

### 法二 快排数组划分

由题目知，我们只需要找出TopK，因此相对于法一，我们不需要对全部元素进行排序，而且对k个元素的顺序也没有要求，所以对快排进行优化，使其不用排序整个数组。

步骤：

1. **哨兵划分**：
   - 通过一趟排序，对数组进行划分，基准数为arr[i]，左数组区间为[$l$ ,$i-1$]，右数组区间为[$i+1$ ，$r$]。
2. **递归或返回**：（$k+1$代表的是前k小后的那一个数字）
   - 若$k < i$，代表第$k+1$小的数字在左子数组中，则递归左子数组
   - 若$k > i$，代表第$k+1$小的数字在右子数组中，则递归右子数组
   - 若$k = i$，代表arr[k]为第$k+1$小的数字，则返回数组前k个数字即可

```javascript
var getLeastNumbers = function(arr, k){
  if(k >= arr.length) return arr;
  return quickSort(arr, k, 0, arr.length - 1);
}
function quickSort(arr, k, l, r){
    let i = l, j = r;
    while(i < j){
      while(i < j && arr[j] >= arr[l]) j--;
      while(i < j && arr[i] <= arr[l]) i++;
      [arr[i],arr[j]] = [arr[j],arr[i]];
    }
    [arr[i],arr[l]] = [arr[l],arr[i]];
    if(i > k) return quickSort(arr, k, l, i - 1);
    if(i < k) return quickSort(arr, k, i + 1, r);
    return arr.slice(0, k);
  }
```

- 时间复杂度：O($N$)
- 空间复杂度：O($logN$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

