## 重建二叉树

> 剑指Offer 07.重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

限制：0 <= 节点个数 <= 5000

## 题解

首先我们要明白前序遍历和中序遍历的节点遍历顺序。

前序遍历：根->左->右

中序遍历：左->根->右

根据题目我们可以得到：

![image.png](https://pic.leetcode-cn.com/1615303546-KEFIrv-image.png)

前序遍历的根节点，在中序遍历中它的位置左侧是左子树，右侧是右子树。在中序遍历获取的索引中可以帮助我们划分前序数组，所以我们可以通过得到的数组递归得到叶子节点，最后通过回溯的方法，组装成一棵完整的树。

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder){
  if(preorder.length === 0) return null
  const cur = new TreeNode(preorder[0]) // 获取根节点
  const index = inorder.indexOf(preorder[0]) // 获取中序数组中根节点的位置
  cur.left = buildTree(preorder.slice(1,index+1),inorder.slice(0,index))
  cur.right = buildTree(preorder.slice(index+1),inorder.slice(index+1))
	return cur
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～