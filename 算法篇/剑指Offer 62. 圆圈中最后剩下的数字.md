## 圆圈中最后剩下的数字

> 剑指Offer 62. 圆圈中最后剩下的数字
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/

0，1，...，n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

示例1：

```
输入: n = 5, m = 3
输出: 3
```

示例2：

```
输入: n = 10, m = 17
输出: 2
```

限制：

- 1 <= n <= 10^5
- 1 <= m <= 10^6

## 题解

### 动态规划

本质上这道题就是一个约瑟夫环。n个人编号0～n-1，每数m次删掉一个人。使用$f(n)$表示n个人最终剩下人的编号。

n个人删掉的第一个人的编号是$(m-1)\%n$，其之后的人编号为$(m-1+1)\%n$即$m\%n$是下一个约瑟夫环的编号为0的那个人，n-1个人时编号为$i$的人就是n个人时$(m+i)\%n$。

因此得出$f(n)=(m+f(n-1))\%n$，1个人时只有一个编号0，$f(1)=0$。

因此动态规划转移方程为：
$$
dp[i] = (dp[i-1]+m)\%i
$$
代码如下：

```javascript
/**
 * @param {number} n
 * @param {number} m
 * @return {number}
 */

var lastRemaining = function (n, m) {
  let ans = 0;
  for (let i = 2; i <= n; i++) {
    ans = (ans + m) % i;
  }
  return ans;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～