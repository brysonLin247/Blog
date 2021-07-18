## 二叉搜索树的后序遍历序列

> 剑指Offer 33.二叉搜索树的后序遍历序列
>
> 难度：中等

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回true，否则返回false。假设输入的数组的任意两个数字都互不相同。

参考以下这棵二叉搜索树：

```
     5
    / \
   2   6
  / \
 1   3
```

示例1：

```
输入: [1,6,3,2,5]
输出: false
```

示例2：

```
输入: [1,3,2,6,5]
输出: true
```

提示：数组长度 <= 1000

## 题解

二叉搜索树特性：**左子树均小于根节点，右子树均大于根节点**。

依照这个特性，判断二叉搜索树的后序遍历序列是否为true，只需要判断左子树是否均小于根节点，右子树是否均大于根节点。

### 递归法

```javascript
/**
 * @param {number[]} postorder
 * @return {boolean}
 */
var verifyPostorder = function(postorder) {
  let len = postorder.length; // 获取输入数组的长度
  if(len < 2) return true; // 若为叶子节点，则返回true
  let root = postorder[len - 1]; // 后序遍历的最后一个元素为根节点
  let index = 0;
  for(let i = 0;i < len - 1;i++){ // len - 1是为了最后一个根节点
    if(postorder[i] > root){
      break;
    }
    index++; // 循环中如果某个节点大于根节点，说明该位置后的节点均为右子树的节点，此时需要记录该点的位置，用于划分左/右子树
  }
  // 需要判断右子树中的所有元素是否都大于根节点，返回boolean值
  let result = postorder.slice(index,len - 1).every(x => x > root);
  if(result){
    return verifyPostorder(postorder.slice(0,index - 1)) && verifyPostorder(postorder.slice(index,len - 1));
  } else {
    return false;
  }
};
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

