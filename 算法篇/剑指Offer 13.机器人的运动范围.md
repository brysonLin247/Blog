## 机器人的运动范围

> 剑指Offer 13. 机器人的运动范围

地上有一个m行m列的方格，从坐标 [0, 0] 到坐标 [m-1, n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35,37]，因为 3+5+3+7=18。但它不能进入方格 [35,38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

示例1:

```
输入：m = 2, n = 3, k = 1
输出：3
```

示例2：

```
输入：m = 3, n = 1, k = 0
输出：1
```

提示：

- 1 <= n,m <= 100
- 0 <= k <= 20

## 题解

### 法一 BFS

看到题目中有“向x移动”之类的字眼就会想到BFS。而且必有除方格临界值外的限制条件：“行坐标和列坐标的数位之和大于k”。

根据题意大致可以划分以下几个关键点：

- 位数和的计算
- 四周方向的遍历
- 限制条件
- 格子数的统计

### 逻辑梳理

#### 位数和的计算

1. 利用字符串

   将数值划分成由位数组成的数组，然后累加每个位数上的值。

```javascript
function getSum(num){
	let stringAry = num.toString().split('');
	return stringAry.reduce((a,b)=>Number(a)+Number(b),0);
}
```

2. 数学公式
   - 对数值进行基于10的取余，得到该位数上的值
   - 更新数值（更新后的数值 = 更新前的数值/10），就移除了之前的位数
   - 对更新后的数值进行相同的操作（取余，更新）
   - 当数值为0时，得出计算结果

```javascript
function getSum(num){
	let answer = 0;
	while(num){
		answer += num % 10;
		// 向下取整，因为可能出现小数
		num = Math.floor(num/10);
	}
	return answer;
}
```

#### 四周方向的遍历

可以使用 **方向数组** 来进行遍历辅助。

```javascript
// 方向数组
const directionAry = [
    [-1, 0], // 上
    [0, 1], // 右
    [1, 0], // 下
    [0, -1] // 左
];
```

本题可以简化成“**向右**”和“**向下**”的遍历操作

#### 限制条件

单元格坐标 [ i, j ]不能超过其方格的边界

- i >= 0
- j >= 0
- i < m
- j < m
- getSum(offsetX) + getSum(offsetY) > k（题目规定）

此外，**已经到达过的单元格，是不需要再纳入到到达个数统计到范畴内的**，最便捷的做法就是使用Set，Set这一数据结构满足了 **项的唯一性**。

#### 格子数的统计

利用Set的优点即可。

#### 代码实现

```javascript
/**
 * @param {number} m
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
var movingCount = function(m, n, k) {
    function getSum(num){
      let answer = 0;
      while(num){
        answer += num % 10;
        num = Math.floor(num / 10);
      }
      return answer;
    }
  // 方向数组
  const directionAry = [
    [-1,0], // 上
    [0,1],  // 右
    [1,0],  // 下
    [0,-1], // 左
  ];
  
  // 已经走过的坐标
  let set = new Set(['0,0']);
  
  // 遍历的坐标队列，题目要求从[0,0]开始走
  let queue = [[0,0]];
  
  // 遍历队列中的坐标
  while(queue.length){
    // 移除队首坐标
    let [x,y] = queue.shift();
    
    // 遍历方向
    for(let i=0;i<4;i++){
      let offsetX = x + directionAry[i][0];
      let offsetY = y + directionAry[i][1];
      
      // 临界值判断
      if(offsetX < 0 || offsetX >= m || offsetY < 0 || offsetY >= n || getSum(offsetX) + getSum(offsetY) > k || set.has(`${offsetX},${offsetY}`)){
        continue;
      }
      
      // 走过的格子就不再纳入统计
      set.add(`${offsetX},${offsetY}`);
      
      // 将该坐标加入队列（因为这个坐标的四周没有走过，需要纳入下次的遍历）
      queue.push([offsetX,offsetY]);
    }
  }
  // 走过坐标的个数就是可以到达的格子数
  return set.size;
};
```

### 法二 DFS

```javascript
/**
 * @param {number} m
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
var movingCount = function(m,n,k){
  function getSum(num){
      let answer = 0;
      while(num){
        answer += num % 10;
        num = Math.floor(num / 10);
      }
      return answer;
    }
  
  const directedArr = [
    [1,0], // down
    [0,1]  // right
  ];
  
  var set = new Set(['0,0']);
  dfs(0,0,k);
  
  function dfs(x,y,k){
    for(let i = 0;i < 2;i++){
      let offsetX = x + directedArr[i][0];
      let offsetY = y + directedArr[i][1];
      if(offsetX<0 || offsetX>m-1 || offsetY<0 || offsetY>n-1 || getSum(offsetX) + getSum(offsetY) > k || set.has(`${offsetX},${offsetY}`)){
        continue;
      }
      set.add(`${offsetX},${offsetY}`);
      dfs(offsetX,offsetY,k);
    }
  }
  return set.size;
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

