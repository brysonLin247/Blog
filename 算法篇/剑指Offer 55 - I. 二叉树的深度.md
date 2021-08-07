##  二叉树的深度

> 剑指Offer 55 - I. 二叉树的深度
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点一次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

例如：

给定二叉树`[3,9,20,null,null,15,7]`，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度3。

提示：节点总数 <= 10000

## 题解

### 法一 DFS

采用后序遍历的**递归**方式，此树的深度等于左子树深度 与右子树深度的最大值+1（根节点）。

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxDepth = function (root) {
  return root === null ? 0 : Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
};
```

采用**栈**的实现方式如下：

```javascript
var maxDepth = function (root) {
  if (!root) return 0;
  let res = 0;
  const stack = [ // 将节点与深度放一起
    [root, 0]
  ];

  while (stack.length) {
    const [node, p] = stack.pop();
    res = Math.max(res, p + 1)；

    node.left && stack.push([node.left, p + 1]);
    node.right && stack.push([node.right, p + 1]);
  }
  return res;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

### 法二 BFS

按层序遍历，使用**队列**实现。

步骤如下：

- 将root放入队列queue
- 创建res用于计算深度
- 检查queue是否为空
  - 不为空，则res+1，依次遍历当前queue内所有节点，检查每个节点的左右子节点，如果左右子节点不为空，则继续将其放入queue，继续循环
  - 为空，则跳出循环
- 返回res

```javascript
var maxDepth = function (root) {
  if (!root) return 0;
  let queue = [root],
    res = 0;
  while (queue.length) {
    res++;
    let nodeNum = queue.length; // 第res层的节点数量
    while (nodeNum--) {
      const node = queue.shift(); // 返回queue第一个元素并删除
      node.left && queue.push(node.left);
      node.right && queue.push(node.right);
    }
  }
  return res;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

