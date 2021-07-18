## 二叉树的镜像

> 剑指Offer 27.二叉树的镜像
>
> 难度：简单

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

镜像输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

示例1：

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

限制：0 <= 节点个数 <= 1000

## 题解

### 递归法

镜像，即从上到下，依次交换每个节点的左右节点。

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
 * @return {TreeNode}
 */
var mirrorTree = function(root){
  if(!root) return null;
  // 交换当前节点的左右节点
  const leftNode = root.left; // 声明临时变量存储左节点
  root.left = root.right;
  root.right = leftNode;
  
  // 对左右子树做相同操作
  mirrorTree(root.left);
  mirrorTree(root.right);
  return root;
}

或
/*
* 声明临时变量temp存储root的左子节点left
* 递归右子节点，将返回值赋给root的左子节点
* 递归左子节点，将返回值赋给root的右子节点
*/
var mirrorTree = function(root){
  if(!root) return root;
  var temp = root.left;
  root.left = mirrorTree(root.right);
  root.right = mirrorTree(temp);
  return root;
}

或

var mirrorTree = function(root){
  if(!root) return null;
  [root.left,root.right] = [mirrorTree(root.right),mirrorTree(root.left)];
  return root;
}

或

var mirrorTree = function(root) {
    return root == null ? null : new TreeNode(root.val, mirrorTree(root.right), mirrorTree(root.left));
};
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

