## 从上到下打印二叉树

> 剑指Offer 32 - I.从上到下打印二叉树
>
> 难度：中等

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

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
[3,9,20,15,7]
```

提示：节点总数 <= 1000

## 题解

### 层序遍历

使用一个队列来存储有用的节点。步骤如下：

- 将root放入队列
- 取出队首元素，将val放入返回的数组中
- 取出队首元素的子节点，若不为空，则将子节点放入队列
- 检查队列是否为空，为空则返回数组；否则，回到第二步

JS中的队列都是使用数组来实现：

- 入队：queue.push(val)
- 出队：queue.shift()
- 检查是否为空：!!queue.length
- 查看队首元素：queue[0]

```javascript
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
var levelOrder = function(root){
  if(!root){
    return [];
  }
  const data = [];
  const queue = [root]; // 队列
  while(queue.length){
    const head = queue.shift(); // 出队
    data.push(head.val);
    head.left && queue.push(head.left); // 入队
    head.right && queue.push(head.right);// 入队
  }
  return data;
}
```

时间复杂度O(n)、空间复杂度O(n)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

