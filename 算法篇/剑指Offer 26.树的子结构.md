## 树的子结构

> 剑指Offer 26.树的子结构
>
> 难度：中等

输入两棵二叉树A和B，判断B是不是A的子结构。（约定空树不是任意一个树的子结构）。

B是A的子结构，即A中有出现和B相同的结构和节点值。

例如：

给定的树A：

```
     3
    / \
   4   5
  / \
 1   2
```

给定的树B：

```
   4 
  /
 1
```

返回true，因为B与A的一个子树拥有相同的结构和节点值。

示例1：

```
输入：A = [1,2,3], B = [3,1]
输出：false
```

示例2：

```
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```

限制：0 <= 节点个数 <= 10000

## 题解

### 递归法

步骤：

- 判断A和B是否为空，为空则返回false
- 比较当前节点值或者A的左右子树的值相对于B的值（A的根节点和B的根节点不同的情况）
- 递归比较左右节点的值，直到遍历完B树。

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * @param {TreeNode} A
 * @param {TreeNode} B
 * @return {boolean}
 */
var isSubStructure = function(A,B){
  // 约定空树不是任意一个树的子结构
  if(!A || !B){
    return false;
  }
  return isSameTree(A,B) || isSubStructure(A.left,B) || isSubStructure(A.right,B)
};

var isSameTree = function(A,B){
  // B子树是空子树 ok
  if(!B){
    return true;
  }
  // A子树是空子树且B非空，不ok
  if(!A){
    return false;
  }
  // 当前节点的值不相等，不ok
  if(A.val !== B.val){
    return false;
  }
  // 递归考察左子树、右子树
  return isSameTree(A.left,B.left) && isSameTree(A.right,B.right);
}
```

- 时间复杂度：最坏情况下为O(m*n)，其中m为A树的节点数，n为B树的节点数
- 空间复杂度：最坏情况下就是一条直的树枝没有分叉，为B的高度n，即O(n)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

