## 把数字翻译成字符串

> 剑指Offer 46.把数字翻译成字符串
>
> 难度：中等
>
> 题目：https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/

给定一个数字，我们按照如下规则把它翻译为字符串：0翻译成“a”，1翻译成“b”，......，11翻译成“l”，......，25翻译成“z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

示例1：

```
输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
```

提示：0 <= num < 2^31

## 题解

### 法一 动态规划

思路：

定义dp[i]为翻译前i个数的方法数，翻译前0个数将dp[0]设为1，以满足动态规划需要。

这道题的转移方程为：
$$
dp[i]=\begin{cases} dp[i-1]，\quad\quad\quad\quad\quad str[i]和str[i−1]不能合成一个字符\\ dp[i-1]+dp[i-2]\quad str[i]和str[i−1]能够合成一个字符\end{cases}
$$

```javascript
/**
 * @param {number} num
 * @return {number}
 */
var translateNum = function (num) {
  const str = num.toString();
  const len = str.length;
  const dp = new Array(len + 1);
  dp[0] = 1, dp[1] = 1;
  for (let i = 2; i < len + 1; i++) {
    const temp = Number(str[i - 2] + str[i - 1]);
    if (temp >= 10 && temp <= 25) {
      dp[i] = dp[i - 1] + dp[i - 2];
    } else {
      dp[i] = dp[i - 1];
    }
  }
  return dp[len];
};
```

- 时间复杂度：O($N$)，N为状态转移的次数
- 空间复杂度：O($N$)，空间复杂度还能继续优化，不使用dp这个数组去存储结果，而是用两个变量去存dp项，优化如下：

```javascript
var translateNum = function (num) {
  const str = num.toString();
  let pre = 1,
    cur = 1;
  for (let i = 2; i < str.length + 1; i++) {
    const temp = Number(str[i - 2] + str[i - 1]);
    if (temp >= 10 && temp <= 25) {
      const t = cur; // 保存上个状态，即dp[i - 1]
      cur = pre + cur; // dp[i] = dp[i - 2] + dp[i - 1]
      pre = t; // 更新上上个状态，即dp[i - 2] = dp[i - 1];
    } else {
      pre = cur; // 将dp[i - 2]更新为dp[i - 1]，即dp[i] = dp[i - 1]，cur不用改变
    }
  }
  return cur;
};
```

- 时间复杂度：O($N$)，N为状态转移的次数
- 空间复杂度：O($1$)

### 法二 数字求余

思路：

- 利用求余数运算num%10和求整运算num / 10，可以获得数字num的各位数字（获取顺序为个位、十位、百位...） 。
- 通过**求余**和求整方式实现**从右向左**的遍历运算。

转移方程为：
$$
dp[i]=\begin{cases} dp[i+1]，\quad\quad\quad\quad\quad tmp  \in  [ 0,10) \cup (25,99 ] \\ dp[i+1]+dp[i+2]\quad tmp \in [ 10,25 ]\end{cases}
$$

```javascript
var translateNum = function (num) {
  let a = 1,
    b = 1,
    x, y = num % 10; // b在a的后面，y在x的后面
  while (num) {
    num = Math.floor(num / 10);
    x = num % 10;
    let tmp = 10 * x + y; // tmp代表的是从右到左一直组合成两位数的那个数，用于判断
    let c = (tmp >= 10 && tmp <= 25) ? a + b : a;
    b = a;
    a = c;
    y = x;
  }
  return a;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

### 法三 DFS

这道题抽象可以理解为树模型，即**求一棵二叉树从根节点到达叶子结点的路径总数**。

```javascript
var translateNum = function (num) {
  const str = num.toString();

  const dfs = (str, pos) => { // pos用于指向某个字符
    let len = str.length;
    if (pos >= len - 1) return 1;
    const temp = Number(str[pos] + str[pos + 1]);
    if (temp >= 10 && temp <= 25) {
      return dfs(str, pos + 1) + dfs(str, pos + 2);
    }
    return dfs(str, pos + 1);
  }

  return dfs(str, 0);
}
```

- 时间复杂度：O($2^N$)，此方法递归时，会产生重复子树，因此我们可以进行优化。
- 空间复杂度：O($N$)

### 法四 DFS + 备忘录

DFS中会产生重复的子树，当我们在优先递归左子树时，若在右侧遇到重复子树，没有必要重新计算。因此，我们需要将计算过的结果用一个备忘录记录下来，再遇到就直接拿来用。这里我们用map来作备忘录。

```javascript
var translateNum = function (num) {
  const str = num.toString();
  const len = str.length;
  const map = new Map();
  map.set(len - 1, 1); // 叶子节点的父节点，其pos指向的是最后一个
  map.set(len, 1); // 叶子节点的值

  const dfs = (str, pos, map) => {
    if (map.get(pos)) return map.get(pos); // 若有重复，则直接发挥重复的。
    const temp = Number(str[pos] + str[pos + 1]);
    if (temp >= 10 && temp <= 25) {
      map.set(pos, dfs(str, pos + 1, map) + dfs(str, pos + 2, map));
    } else {
      map.set(pos, dfs(str, pos + 1, map));
    }
    return map.get(pos);
  }

  return dfs(str, 0, map);
}
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

