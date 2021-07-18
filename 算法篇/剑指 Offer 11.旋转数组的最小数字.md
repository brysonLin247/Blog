## 旋转数组的最小数字

> 剑指Offer 11.旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组【3，4，5，1，2】为【1，2，3，4，5】的一个旋转，该数组的最小值为1。

示例1:

```
输入：[3,4,5,1,2]
输出：1
```

示例2：

```
输入：[2,2,2,0,1]
输出：0
```

## 题解

### 爆破法

根据题意，我们第一时间就能通过爆破解决，用一个变量记录一下当前遍历过程中遇到的最小值是多少，然后遍历结束后，返回最小值即可。

但这样子的爆破不够好，因为其实数组是被旋转的，所以，在遍历过程中，只要有小于numbers[0]，那么该数字一定是最小值。

```javascript
function minArray(numbers){
  for(let i=0;i<numbers.length;i++){
    if(numbers[i]<numbers[0]){
      return numbers[i]
    }
  }
  return numbers[0]
}
```

但爆破不是我们的目标，我们目的应该是降低算法的复杂度，所以采用二分法更好。

### 二分法

首先，创建两个指针left、right分别指向numbers首尾数字，然后计算出两指针之间的中间索引值middle，然后我们会遇到以下三种情况：

1. middle > right：代表最小值一定在middle右侧，所以left移到middle+1的位置。
2. middle < right：代表最小值一定在middle左侧或者就是middle，所以right移到middle到位置。
3. middle既不大于left指针，也不小于right指针的值，代表着middle可能等于left指针的值，或者right指针的值，我们这时候只能让right指针递减，来一个一个找最小值了。

```javascript
/**
 * @param {number[]} numbers
 * @return {number}
 */
var minArray = function(numbers){
  let left = 0, right = numbers.length-1;
  while(left < right){
    let middle = left + ~~((right - left) / 2); // 避免（left+right）出现溢出
    if(numbers[middle] > numbers[right]){
      left = middle + 1;
    }else if(numbers[middle] < numbers[right]){
      right = middle;
    }else{
      right--;
    }
  }
  return numbers[left];
}
```



****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

