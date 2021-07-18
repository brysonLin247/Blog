> 剑指Offer 04.二维数组中的查找

## 二维数组中的查找

在一个n*m的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

🌰 现有矩阵matrix如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

给定target = 5，返回true；给定target=20，返回false。

限制：0 <= n <= 1000; 0 <= m <= 1000

## 题解

### 法一 坐标轴法

根据题意知，二维数组从左往右递增，从上往下递增，则：

- 某列的某个数字，该数之上的数字，都比其小；
- 某行的某个数字，该数右侧的数字，都比其大；

所以，解题流程如下：

1. 以二维数组左下角为原点，建立直角坐标轴。
2. 若当前数字大于了查找数，查找往上移一位。
3. 若当前数字小于了查找树，查找往右移一位。

如果target = 19，则路径为：**18->21->13->14->17->24->22->19**

```javascript
var findNumberIn2DArray = function(matrix,target){
  if(!matrix.length) return false;
  let x = matrix.length-1 , y = 0;
  while(x>=0 && y< matrix[0].length){
    if(matrix[x][y] === target){
      return true;
    }else if(matrix[x][y] > target){
      x--;
    }else{
      y++;
    }
  }
  return false;
}
```

结果：

![image-20210623012353641](/Users/brysonlin/Library/Application Support/typora-user-images/image-20210623012353641.png)



### 法二 数组降维法

将二维数组扁平化，再看其是否包含目标值。

```javascript
var findNumberIn2DArray = function(matrix,target){
  return matrix.flat(Infinity).includes(target);
}
```

结果：

![image-20210623013349164](/Users/brysonlin/Library/Application Support/typora-user-images/image-20210623013349164.png)



### 暴力法

这里不做过多介绍。

