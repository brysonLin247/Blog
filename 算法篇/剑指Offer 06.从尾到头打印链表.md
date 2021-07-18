## 从尾到头打印链表

> 剑指Offer 06.从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值。（用数组返回）。

示例：

```javascript
输入：head = [1,3,2]
输出：[2,3,1]
```

限制： 0 <= 链表长度 <= 10000

## 题解

### 法一 常规解法

遍历链表、将每个元素从数组头部插入、实现倒序输出。

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
 * @return {number[]}
 */

var reversePrint = function(head){
  let nums = [];
  let node = head;
  while(node !== null){
    nums.unshift(node.val);
    node = node.next;
  }
  return nums;
}
```

### 法二 递归法

递归思路：递归本身与栈的后进先出的原理一致，通过递归从最后一个结果开始保存到数组中，实现倒序打印。

这种方法需要注意是否会因为链表过长导致栈溢出。

```javascript
var reversePrint = function(head){
  let nums = []
  const visit = function(head){
    if(head!==null){
      visit(head.next)
      nums.push(head.val)
    }
  };
  visit(head) // 首次进入
  return nums
}
```

### 法三 反转链表法

先将链表反转，后对反转的链表进行遍历输出

```javascript
var reversePrint = function(head){
  if(head === null || head.next === null) return head;
  let p = head.next;
  head.next = null;
  let tmp = null;
  while(p!==null){
    tmp = p.next
    p.next = head
    head = p
    p = tmp
  }
  return head // 返回的是一个链表，而非数组，这里就不做存数组的操作了
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

