## 礼物的最大价值

> 剑指Offer 47.礼物的最大价值
>
> 难度：中等
>
> 题目：https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/

在一个m*n的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

示例1：

```
输入: 
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
```

提示：

- 0 < grid.length <= 200
- 0 < grid[0].length <= 200

## 题解

由题意知，只能向下走和向右走，设f(i ,j)表示从[i, j]位置开始能收集到的最大礼物，则可以得出公式：

`f(i, j) = grid[i][j] + Math.max(f(i + 1, j), f(i, j + 1))`

因此可以采用递归方式解决。

### 法一 递归

这种方法会超时。

```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var maxValue = function (grid) {
  let rowLen = grid.length;
  if (!rowLen) return 0;
  let colLen = grid[0].length;
  const recurse = (i, j) => {
    if (i >= rowLen || j >= colLen) return 0;
    return grid[i][j] + Math.max(recurse(i + 1, j), recurse(i, j + 1));
  }
  return recurse(0, 0);
};
```

### 法二 递归 + 备忘录

递归会产生重复的路径，我们没有必要去做重复的计算，因此可以使用map将计算过的结果记录下来，再次遇到时直接拿来用就可以。

```javascript
var maxValue = function (grid) {
  let rowLen = grid.length;
  if (!rowLen) return 0;
  let colLen = grid[0].length;
  let map = new Map();
  let tmp = 0;

  const recurse = (i, j) => {
    if (i >= rowLen || j >= colLen) return 0;
    if (map.has(i * colLen + j)) return map.get(i * colLen + j)
    tmp = grid[i][j] + Math.max(recurse(i + 1, j), recurse(i, j + 1));
    map.set(i * colLen + j, tmp);
    return tmp;
  }
  return recurse(0, 0);
};
```

### 法三 二维动态规划

可以使用dp二维数组去进行动态规划

```javascript
var maxValue = function(grid) {
  let rows = grid.length;
  if(!rows) return 0;
  let cols = grid[0].length;
  let dp = Array(rows).fill(0).map(() => Array(cols).fill(0));
  
  // 从右下角到左上角
  for(let i = rows - 1; i >= 0; i--){ 
    for(let j = cols - 1; j >= 0; j--){
      let bottom = i + 1 < rows ? dp[i+1][j] : 0;
      let right = j + 1 < cols ? dp[i][j+1] : 0;
      dp[i][j] = grid[i][j] + Math.max(bottom, right);
    }
  }
  return dp[0][0];
}
```

- 时间复杂度：O($MN$)
- 空间复杂度：O($MN$)

### 法四 一维动态规划

这里的dp数组我们可以将它优化成一维的，优化之后的一维dp保存的是当前上一行的最大价值，然后我们从做到有去更新这个数组即可：

```javascript
var maxValue = function(grid) {
  let rows = grid.length;
  if(!rows) return 0;
  let cols = grid[0].length;
  let dp = Array(cols).fill(0);
  
  for(let i = rows - 1; i >= 0; i--){
    for(let j = cols - 1; j >= 0; j--){
      let bottom = i + 1 < rows ? dp[j] : 0;
      let right = j + 1 < cols ? dp[j+1] : 0;
      dp[j] = grid[i][j] + Math.max(bottom, right);
    }
  }
  return dp[0];
}
```

- 时间复杂度：O($MN$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

