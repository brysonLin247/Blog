## 矩阵中的路径

> 剑指Offer 12.矩阵中的路径

给定一个m x n二维字符网格board和一个字符串单词word。如果word存在于网格中，返回true；否则，返回false。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

例如，在下面的3x4的矩阵中包含单词“ABCCED”（单词中的字母已标出）。

![img](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

示例1:

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

示例2:

```
输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```

提示：

- 1 <= board.length <= 200
- 1 <= board[i].length <= 200
- board 和 word 仅由大小写英文字母组成

## 题解

典型的矩阵搜索问题，可使用**深度优先搜索（DFS）+ 剪枝**解决。

- **深度优先搜索：**可以理解为暴力法遍历矩阵中所有字符串可能性。DFS通过递归，先朝一个方向搜索到底，再回溯至上个节点，沿另一个方向搜索，以此类推。
- **剪枝：**在搜索中，遇到`这条路不可能和目标字符串匹配成功`的情况（例如：此矩阵元素和目标字符不同、此元素已被访问），则应立即返回，称之为`可行性剪枝`。

![Picture0.png](https://pic.leetcode-cn.com/1604944042-glmqJO-Picture0.png)

#### DFS解析：

- **递归参数**：当前元素在矩阵board中的行列索引 x 和 y，当前目标字符在word中的索引 k。
- **终止条件：**
  1. 返回false：
     - 行或列索引越界
     - 当前矩阵元素与目标字符不同
     - 当前矩阵元素已被访问
  2. 返回true：
     - k === word.length - 1，即字符串word已全部匹配。
- **递推工作：**
  1. 标记当前矩阵元素：将board[ x ] [ y ] 修改为字符串 ‘ - ’，代表此元素已访问过，防止之后搜索时重复访问。
  2. 搜索下一单元格：朝当前元素的 **上、右、下、左** 四个方向开启下层递归，使用 **或** 连接（代表只需找到一条可行路径就直接返回，不再做后续DFS），并记录结果至 res。
  3. 还原当前矩阵元素：将board [ i ] [ j ]元素还原至初始值 

```javascript
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
var exist = function(board, word){
  var xLen = board.length;
  var yLen = board[0].length;
  var k = 0;
  
  var dfs = function(x,y,k){
    if(x < 0 || x >= xLen || y < 0 || y >= yLen || board[x][y] !== word[k])
      return false;
    if(k === word.length - 1) // word 遍历完了
      return true;
    let temp = board[x][y]; // 记录board的值
    board[x][y] = '-';      // 
    let res = dfs(x - 1, y, k + 1) || dfs(x, y + 1, k + 1) || dfs(x + 1, y,  k + 1) || dfs(x, y - 1,  k + 1); // 上 右 下 左
    board[x][y] = temp;
    return res;
  }
  
    for(let i = 0; i < xLen; i++){
      for(let j = 0; j < yLen; j++){
        if(dfs(i,j,k)) return true;
      }
    }
  return false;
}
```



****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

