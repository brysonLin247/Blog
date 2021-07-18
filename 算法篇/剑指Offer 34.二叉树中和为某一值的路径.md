## 二叉树中和为某一值的路径

> 剑指Offer 34.二叉树中和为某一值的路径
>
> 难度：中等

输入一棵二叉树和整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

示例：

给定如下二叉树，以及目标和target = 22。

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

返回：

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

提示：数组长度 <= 10000

## 题解

### 法一 前序遍历

步骤：

- 找路径时，每次都将新的节点放入当前保存的路径（stack）
- 检查新节点是否为叶子节点（题目中说要从根节点一直到叶子节点，故需要做判断）
  - 是叶子节点：
    - 路径节点之和是否等于传进来的target，是的话则将路径存入结果
  - 不是叶子节点：继续遍历左子树和右子树

这个流程是一个前序遍历，在遍历的过程中，同时还维护了当前路径与总和的信息。

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} target
 * @return {number[][]}
 */
var pathSum = function(root,target){
  var result = [];
  if(root){
    FindPath(root,target,[],0,result); 
  }
  return result;
}

function FindPath(node,target,stack,sum,result){
  stack.push(node.val);
  sum += node.val;
  if(!node.left && !node.right && sum === target){
    result.push(stack.slice(0)); //stack.slice(0)深拷贝
  }
  if(node.left){
    FindPath(node.left,target,stack,sum,result);
  }
  if(node.right){
    FindPath(node.right,target,stack,sum,result);
  }
  stack.pop();
}
```

### 法二 非递归前序遍历

借助栈，空间换取时间。

这里我们将栈中的元素定义为（node，节点路径组成的数组）这样的元祖。

步骤：

- 取出栈顶
- 若当前节点为叶子节点，且路径和等于target，则将路径存入结果
- 如果左右节点非空，则需要对应不同参数放入栈中

```javascript
var pathSum = function(root,target){
  if(!root) return [];
  const res = [], calcSum = x => x.reduce((a,b) => a + b), stack = [[root,[root.val]]];
  while(stack.length){
    const [node,prev] = stack.pop();
    if(!node.left && !node.right && calcSum(prev) === target){
      res.push(prev);
    }
    node.left && stack.push([node.left,[...prev,node.left.val]]);
    node.right && stack.push([node.right,[...prev,node.right.val]]);
  }
  return res;
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

