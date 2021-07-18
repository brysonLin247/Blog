## 合并两个排序的链表

> 剑指Offer 25.合并两个排序的链表
>
> 难度：简单

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

示例1：

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

限制：0 <= 链表长度 <= 1000

## 题解

### 法一 递归法

递归 分治剪枝：返回有效期望链表项

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
	if(l1 === null) return l2;
  if(l2 === null) return l1;
  if(l1.val < l2.val){
    l1.next = mergeTwoLists(l1.next,l2);
    return l1;
  }else{
    l2.next = mergeTwoLists(l1,l2.next);
    return l2;
  }
};
```

时间复杂度：O(m+n)，空间复杂度O(m+n)

### 法二 迭代法

设置哨兵节点preNode，流程如下

- l1和l2均没遍历完：
  - 如果l1.val > l2.val，当前node的next指向l2，移动l2指针。
  - 否则，当前node的next指向l1，移动l1指针。
  - 移动node指针
  - 继续循环
- 否则，结束循环：
  - 如果l1未遍历完，node的next指向l1
  - 如果l2未遍历完，node的next指向l2

```javascript
var mergeTwoLists = function(l1,l2){
  if(!l1){
    return l2;
  }else if(!l2){
    return l1;
  }
  let preNode = new ListNode(-1);
  let node = preNode;
  while(l1 && l2){
    if(l1.val > l2.val){
      node.next = l2;
      l2 = l2.next;
    }else{
      node.next = l1;
      l1 = l1.next;
    }
    node = node.next;
  }
  if(l1){
    node.next = l1;
  }else if(l2){
    node.next = l2;
  }
  return preNode.next;
}
```

- 时间复杂度O(m+n)，m和n分别为两链表长度

- 空间复杂度O(1)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

