## 平衡二叉树

> 剑指Offer 55 - II. 平衡二叉树
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

示例1：

给定二叉树`[3,9,20,null,null,15,7]`

```
   3
   / \
  9  20
    /  \
   15   7
```

返回true

示例2：

给定二叉树`[1,2,2,3,3,null,null,4,4]`

```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```

返回false

限制：0 <= 树的结点个数 <= 10000

## 题解

此树的深度等于左子树深度 与右子树深度的最大值+1

### 法一 后序遍历

从底至顶返回子树深度，判断当前左右子树的深度差从而来判断是否为平衡二叉树。

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
 * @return {boolean}
 */
var isBalanced = function (root) {
  return height(root) !== -1;
};

function height(root) {
  if (!root) return 0;
  const left = height(root.left);
  const right = height(root.right);
  if (left === -1 || right === -1 || Math.abs(left - right) > 1) { // 不是平衡树，则“剪枝”，直接返回
    return -1;
  }
  return Math.max(left, right) + 1;
}
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

### 法二 前序遍历

从顶至底递归，对于当前遍历到的节点，首先计算左右子树的高度，如果左右子树的高度差是否不超过 1，再分别逼哦安利左右子节点，并判断左右子树是否平衡。

```javascript
var isBalanced = function (root) {
  if (!root) return true;
  // 根左右
  return Math.abs(height(root.left) - height(root.right)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
};

function height(root) {
  if (!root) return 0;
  return Math.max(height(root.left), height(root.right)) + 1;
}
```

- 时间复杂度：O($N^2$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

