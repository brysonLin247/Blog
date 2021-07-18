## 二叉搜索树与双向链表

> 剑指Offer 36.二叉搜索树与双向链表
>
> 难度：中等
>
> 力扣地址：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

为了让您更好地理解问题，以下面的二叉搜索树为例：

![img](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)

我们希望将这个二叉搜索树转化成双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

下面展示了上面的二叉搜索树转换化成的链表。“head”表示指向链表中有最小元素的节点。

![img](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

## 题解

根据二叉搜索树的性质，可以推断出应该采用中序遍历。

### 法一 递归中序遍历

```javascript
/**
 * @param {Node} root
 * @return {Node}
 */
var treeToDoublyList = function(root){
  if(!root) return;
  let head = null;
  let pre = head;
  inorder(root);
  head.left = pre;
  pre.right = head;
  return head;
 /**
 * @param {Node} node
 */
  function inorder(node){
    if(!node) return;
    // 遍历左子树
    inorder(node.left,pre);
    // 遍历当前根节点
    if(!pre){
      // 遍历到最小的节点（最左边的节点），此时节点就是双向链表的head
      head = node;
    }else{
      pre.right = node;
    }
    node.left = pre;
    pre = node;
    // 遍历右子树
    inorder(node.right,pre);
  }
}
```

- 时间复杂度O(N)
- 空间复杂度O(N)

### 法二 非递归中序遍历

利用栈来模拟递归过程

```javascript
var treeToDoublyList = function(root) {
  if(!root) return;
  const stack = [];
  let node = root
  let pre = null;
  let head = null;
  while(stack.length || node){
    if(node){
      stack.push(node);
      node = node.left;
    }else{
      const top = stack.pop();
      if(!pre){
        head = top;
      }else{
        pre.right = top;
      }
      top.left = pre;
      pre = top;
      node = top.right;// 用于遍历右子树
    }
  }
  head.left = pre;
  pre.right = head;
  return head;
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

