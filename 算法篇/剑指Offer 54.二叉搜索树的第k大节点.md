## 二叉搜索树的第k大节点

> 剑指Offer 54.二叉搜索树的第k大节点
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/

给定一棵二叉搜索树，请找出其中第k大的节点。

示例1：

```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```

示例2：

```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4
```

## 题解

一看到二叉搜索树，就应该想到中序遍历！

一开始想到的其实是暴力解决，即将所有节点遍历一遍将值存进Set里面后再进行sort，最后返回第k大，但是发现这种方法并未利用到二叉搜索树的特性，故抛弃这种做法。

二叉搜索树的特点是右子树>根>左子树，而中序遍历是左根右的顺序，因此「求二叉搜索树的第k大节点」可以转换为「这棵树中序遍历倒序的第k个节点」。

中序遍历：

```javascript
function inOrder(root){
  if(!root) return null;
  inOrder(root.left); // 左
  console.log(root.val); // 根
  inOrder(root.right); // 右
}
```

中序遍历倒序：

```javascript
function inOrder(root){
  if(!root) return null;
  inOrder(root.right); // 右
  console.log(root.val); // 根
  inOrder(root.left); // 左
}
```

求第k大节点，需要：

- 在递归遍历时，记录每个节点的序号，例如根节点的右子树为1，根节点为2，...；
- 当递归到第k个时，记录结果并返回，无需往后递归。

题解如下：

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
 * @param {number} k
 * @return {number}
 */
var kthLargest = function (root, k) {
  let num = 0,
    res = null;
  const dfs = function (node) {
    if (!node) return;
    dfs(node.right);
    num++;
    if (num === k) {
      res = node.val;
      return;
    }
    dfs(node.left);
  };
  dfs(root)
  return res;
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～
