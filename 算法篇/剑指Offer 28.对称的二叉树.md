## 对称的二叉树

> 剑指Offer 28.对称的二叉树
>
> 难度：简单

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它就是对称的。

例如：二叉树[1,2,2,3,4,4,3]是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个[1,2,2,null,3,null,3]则不是镜像对称的：

```
    1
   / \
  2   2
   \   \
   3    3
```

示例1：

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

示例2：

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

限制：0 <= 节点个数 <= 1000

## 题解

### DFS

递归检查左右子树，在递归到下一层时，要同时检查左子树的左和右子树的右：check(left.left,right,right) && check(left.right,right.left)

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
var isSymmetric = function(root){
  if(!root) return true;
  var check = function(left,right){
    if(!left && !right) return true; // 左右子树同时为空，则为镜像
    if(!left || !right) return false;// 左右子树有一个为空，则不为镜像
    return left.val === right.val && check(left.left,right.right) && check(left.right,right.left);
  }
  return check(root.left,root.right)
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～