## 从上到下打印二叉树 ||

> 剑指Offer 32 - II.从上到下打印二叉树 II
>
> 难度：中等

从上到下按层打印二叉树，同一层的节点按照从左到右的顺序打印，每一层打印到一行。

例如：给定二叉树：[3,9,20,null,null,15,7]

```
    3
   / \
  9  20
    /  \
   15   7
```

返回：

```
[
  [3],
  [9,20],
  [15,7]
]
```

提示：节点总数 <= 1000

## 题解

### 层序遍历

与上题《剑指Offer 32 - I.从上到下打印二叉树》类似，步骤如下：

- 将root放入队列
- 创建data用于存放返回的结果、level代表层树。
- 检查queue是否为空
  - 不为空，依次遍历当前queue内所有节点，检查每个节点的左右子节点，如果左右子节点不为空，则继续将其放入queue，继续循环
  - 为空，则跳出循环

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
 * @return {number[][]}
 */
var levelOrder = function(root) {
	if(!root) return [];
  const queue = [root];
  const data = []; // 存放遍历结果
  let level = 0; // 代表当前层数
  while(queue.length){
    data[level] = []; // 第level层的遍历结果
    let levelNum = queue.length;// 第level层的节点数量（例子中每层分别的节点数量为1、2、2）
    while(levelNum--){
      const head = queue.shift();
      data[level].push(head.val);
      head.left && queue.push(head.left);
      head.right && queue.push(head.right);
    }
    level++;
  }
  return data;
};
```

时间复杂度O(N)、空间复杂度O(N)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

