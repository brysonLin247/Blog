## 链表中倒数第k个节点

> 剑指Offer 22.链表中倒数第k个节点
>
> 难度：简单

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

示例：

```
给定一个链表: 1->2->3->4->5, 和 k = 2.
返回链表 4->5.
```

## 题解

### 法一 快慢指针法

设置p、q两个指针，让p先走k步，然后p、q一起走，直到p为null。

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
 * @param {number} k
 * @return {ListNode}
 */
var getKthFromEnd = function(head, k) {
	let p = head,q = head;
  let i = 0;
  while(p){
    if(i >= k){
      q = q.next;
    }
    p = p.next;
    i++;
  }
  return i < k ? null : q;
};
```

### 法二 使用栈

```javascript
var getKthFromEnd = function(head,k){
  var stack = []
  var ans = []
  while(head){ // 所有节点入栈
    stack.push(head);
    head = head.next;
  }
  while(k > 0){
    ans = stack.pop();
    k--;
  }
  return ans;
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

