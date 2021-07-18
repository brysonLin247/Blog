## 序列化二叉树

> 剑指Offer 37.序列化二叉树
>
> 难度：困难
>
> leetcode地址：https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/

请实现两个函数，分别用来序列化和反序列化二叉树。

你需要设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列/反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

示例：

![img](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)

```
输入：root = [1,2,3,null,null,4,5]
输出：[1,2,3,null,null,4,5]
```

## 题解

### 法一 BFS

序列化步骤：

- 创建一个结果数组res，然后用一个队列，初始让根节点入列，对出列的节点进行考察：
  - 出列节点为null，将"$"推入res。
  - 出列节点为数值，将值推入res，并将它的左右节点入列。
- 入列、出列...直到队列为空，就遍历完所有节点，res构建完毕，将res转为字符串。

反序列化步骤：

由上诉序列化能得到的序列是`1 2 3 $ $ 4 5 $ $ $ $`，可以看出第一个是根节点，其他节点都是对应左右子节点。

- 字符串先转成数组list，用指针cursor从第二项开始扫描。
- list[0]构建根节点，将根节点入列。
- 节点出列，此时cursor指向左子节点，cursor指向右子节点。如果子节点为"$"，跳过；否则（如果子节点为数值），创建节点，并让父节点对应子节点，还要将该节点入列。

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

/**
 * Encodes a tree to a single string.
 *
 * @param {TreeNode} root
 * @return {string}
 */
var serialize = function(root) {
  const queue = [root];
  let res = [];
  while(queue.length){
    const node = queue.shift(); // 考察出列节点
    if(node){										// 节点不为null，则节点入列，否则将$入列。
      res.push(node.val);
      queue.push(node.left);
      queue.push(node.right);
    }else{
      res.push('$');
    }
  }
  return res.join(',');					// 数组转字符串
};

/**
 * Decodes your encoded data to tree.
 *
 * @param {string} data
 * @return {TreeNode}
 */
var deserialize = function(data) {
	if(data === '$') return null;
  
  const list = data.split(',');		// 序列化字符串split成数组
  
  const root = new TreeNode(list[0]);// 构建根节点
  const queue = [root];						// 根节点入列
  let cursor = 1;									// 初始指向list第二项
  
  while(cursor < list.length){		// 指针越界，即扫完了序列化字符串
    const node = queue.shift();		// 考察出列节点
    
    const leftVal = list[cursor];	// 左节点值
    const rightVal = list[cursor+1];// 右节点值
    
    if(leftVal !== '$'){					// 节点不为null，则：
      const leftNode = new TreeNode(leftVal);// 创建左子节点
      node.left = leftNode;				// 父节点左子树指向左子节点
      queue.push(leftNode);				// 自己是父节点，入列
    }
    if(rightVal !== '$'){						
      const rightNode = new TreeNode(rightVal);
      node.right = rightNode;							
      queue.push(rightNode);
    }
    cursor += 2;
  }
  return root;
};

/**
 * Your functions will be called as such:
 * deserialize(serialize(root));
 */
```

### 法二 DFS

序列化步骤：

- 递归遍历一棵树，再将返回结果拼接。
- 选择采用前序遍历（根｜左｜右）的方式，在遇到null节点时，用"$"来代替。

反序列化步骤：

- 上述序列化的结果为`1 2 $ $ 3 4 $ $ 5 $ $`
- 定义函数buildTree用于还原二叉树，传入序列化字符串转成的list数组。
- 逐个shift出list首项，构建当前子树的根节点，顺着list，构建顺序是根->左->右
  - 如果弹出字符为$，则返回null
  - 如果弹出字符为数值，则创建root节点，并递归构建root的左右子树，最后返回root。

```javascript
const serialize = (root) => {
  if(root === null){
    return '$';
  }
  const left = serialize(root.left);
  const right = serialize(root.right);
  return root.val + ',' + left + ',' + right;
};

const deserialize = (data) => {
  const list = data.split(',');
  
  const buildTree = (list) => {
    const rootVal = list.shift();
    if(rootVal === '$'){
      return null;
    }
    const root = new TreeNode(rootVal);
    root.left = buildTree(list);
    root.right = buildTree(list);
    return root;
  };
  return buildTree(list);
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

