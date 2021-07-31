## 两个链表的第一个公共节点

> 剑指Offer 52.两个链表的第一个公共节点
>
> 难度：简单
>
> 题目：https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/

输入两个链表，找出它们的第一个公共节点。

如下面的两个链表：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

在节点c1开始相交。

示例1：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

示例2：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

示例3：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
```

注意：

- 如果两个链表没有交点，返回null。
- 在返回结果后，两个链表仍然保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足O($N$)时间复杂度，且仅用O($1$)内存。

## 题解

### 法一 哈希表

使用哈希表存储链表节点，先遍历链表headA，将headA的每个节点加入哈希表，再遍历链表headB，判断遍历节点是否在哈希表中：

- 如果当前节点不在哈希表中，则继续遍历下一个
- 如果当前节点在哈希表中，则后面的节点也都会在哈希表中，返回当前节点。

如果链表headB中的所有节点都不在哈希表中，则两个链表不相交，返回null。

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
 var getIntersectionNode = function(headA, headB) {
  const visited = new Set();
  let tmp = headA;
  while(tmp){
    visited.add(tmp);
    tmp = tmp.next;
  }
  tmp = headB;
  while(tmp){
    if(visited.has(tmp)){
      return tmp;
    }
    tmp = tmp.next;
  }
  return null;
};
```

- 时间复杂度：O($M+N$)
- 空间复杂度：O($N$)

### 法二 双指针

使用两个指针node1，node2分别指向两个链表headA，headB的头节点，同时进行遍历，当node1到达headA的末尾时，重新定位到链表headB的头节点；当node2到达链表headB的末尾时，重新定位到链表headA的头节点。当node1和node2相遇时，所指向的节点就是第一个公共节点

```javascript
var getIntersectionNode = function(headA, headB) {
  if(!headA || !headB){
    return null;
  }
  let node1 = headA, node2 = headB;
  while(node1!==node2){
    node1 = !node1 ? headB : node1.next;
    node2 = !node2 ? headA : node2.next;
  }
  return node1;
};
```

- 时间复杂度：O($M+N$)
- 空间复杂度：O($1$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

