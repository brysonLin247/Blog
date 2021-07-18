## 顺时针打印矩阵

> 剑指Offer 29.顺时针打印矩阵
>
> 难度：简单

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

示例1：

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

示例2：

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

限制：

- 0 <= matrix.length <= 100
- 0 <= matrix[i].length <= 100

## 题解

顺时针方向像剥洋葱那样将矩阵一层一层剥掉，其中flag用来切换方向。

```javascript
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
var spiralOrder = function(matrix){
  var res = [];
  var flag = true;
  while(matrix.length){
    // 从左到右
    if(flag){
      // 第一层
      res = res.concat(matrix.shift());
      // ‘现在’的第一层到最后一层的末尾
      for(let i = 0;i < matrix.length;i++){
        matrix[i].length && res.push(matrix[i].pop());
      };
    }else{// 从右到左
      // 最后一层
      res = res.concat(matrix.pop().reverse());
      // '现在'的最后一层到第一层
      for(let i = matrix.length - 1;i > 0;i--){
        matrix[i]/length && res.push(matrix[i].shift());
      }
    }
    flag = !flag;
  }
  return res;
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

