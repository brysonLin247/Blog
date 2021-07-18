## 反转链表

> 剑指Offer 24.反转链表
>
> 难度：简单

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

示例：

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

限制： 0 <= 节点个数 <= 5000

## 题解

### 法一 迭代法

在遍历链表时，将当前节点的next指针改为指向前一个节点。由于节点没有引用其前一个节点，因此必须事先存储其前一个节点。在更改引用前，还需要存储后一个节点。最后返回新的头引用。

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head){
  let prev = null;
  let curr = head;
  while(curr){
    const next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
  }
  return prev;
}
```

时间复杂度：O(n)，空间复杂度O(1)

### 法二 递归法

假设链表为：

$n~1~→…→n~k−1~→n~k~→n~k+1~→…→n~m~→∅$ 

若从节点$n~k+1~$到$n~m~$已经被反转，而我们正处于$n~k~$。

$n~1~→…→n~k−1~→n~k~→n~k+1~←…←n~m$

我们希望$n~k+1~$的下一个节点指向$n~k~$。

所以，$n~k~.next.next = n~k~$。

需要注意的是$n~1~$的下一个节点必须指向$∅$。如果忽略这一点，链表中可能会产生环。

```javascript
var reverseList = function(head){
  if(head === null || head.next === null){
    return head;
  }
  const newHead = reverseList(head.next);
  head.next.next = head;
  head.next = null;
  return newHead;
}
```

时间复杂度：O(n)，n是链表的长度。需要对链表的每个节点进行反转操作。

空间复杂度：O(n)，n是链表长度，空间复杂度取决于递归调用的栈空间，最多为n层。

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

