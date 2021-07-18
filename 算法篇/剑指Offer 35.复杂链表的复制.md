## 复杂链表的复制

> 剑指Offer 35.复杂链表的复制
>
> 难度：中等

请实现`copyRandomList`函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个`next`指针指向下一个节点，还有一个`random`指针指向链表中的任意节点或者`null`。

示例1：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

示例2：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png)

```
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```

示例3：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png)

```
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```

示例4：

```
输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。
```

提示：

- -10000 <= Node.val <= 10000
- `Node.random`为空（null）或指向链表中的节点
- 节点数目不超过1000

## 题解

难点：本题主要是新增了random指针，在复制链表过程中需要新链表各节点的random引用指向，如下：（random无法确定指向）

```javascript
var copyRandomList = function(head) {
  let cur = head;
  let dum = new Node(0), pre = dum;
  while(!cur){
    let node = new Node(cur.val); // 复制节点 cur
    prev.next = node;							// 新链表的 前驱节点->当前节点
    // prev.random = ???					// 无法确定
    cur = cur.next;								// 遍历下一节点
    pre = node;										// 保存当前节点
  }
  return dum.next;
};
```

### 法一 哈希表

利用哈希表的查询特点，构建**原链表节点**和**新链表对应节点**的键值对映射关系，再遍历构建新链表各节点的`next`和`random`引用指向即可。

流程：

1. 若头节点head为空，返回null。
2. **初始化**：新建哈希表，节点cur指向头节点。
3. **复制链表**：
   1. 建立新节点，并向map添加键值对（原cur节点，新cur节点）。
   2. cur遍历至原链表下一节点。
4. **构建新链表的引用指向**：
   1. 构建新节点next和random引用指向。
   2. cur 遍历至原链表下一节点。
5. **返回值**：新链表的头节点。

```javascript
/**
 * // Definition for a Node.
 * function Node(val, next, random) {
 *    this.val = val;
 *    this.next = next;
 *    this.random = random;
 * };
 */

/**
 * @param {Node} head
 * @return {Node}
 */
var copyRandomList = function(head) {
	if(!head) return null;
  const map = new Map(); // 新建哈希表
  let cur = head;  
  while(cur){
    map.set(cur,new Node(cur.val))
    cur = cur.next;
  }
  cur = head;
  
  while(cur){
   	map.get(cur).next = cur.next ? map.get(cur.next) : null;
    map.get(cur).random = cur.random ? map.get(cur.random) : null;
    cur = cur.next;
  }
  return map.get(head);
};
```

- 时间复杂度O(N)：两轮遍历链表

- 空间复杂度O(N)：使用哈希表的额外空间

###  法二 拼接 + 拆分

构建`旧节点1 -> 新节点1 -> 旧节点2 -> 新节点2 -> ...`的拼接链表，如此可以方便旧节点的random指向节点的同时找到新对应新节点的random指向节点。

步骤：

1. 复制各节点，并构建拼接链表
2. 构建各新节点的random指向，当访问cur.random时，对应新节点cur.next的随机指向节点为cur.random.next。
3. 拆分旧新链表，设置pre和cur分别指向旧链表和新链表的头节点，通过遍历，讲pre和cur两条链表拆分开。
4. 返回新链表的头节点res。

```javascript
var copyRandomList = function(head) {
  if(head === null) return null;
  // 1.复制各节点，并构建拼接链表
  while(cur){
    let tmp = new Node(cur.val);
    tmp.next = cur.next;
    cur.next = tmp;
    cur = tmp.next;
  }
  // 2.构建各新节点的random指向
  cur = head;
  while(cur){
    if(cur.random){
      cur.next.random = cur.random.next;
    }
    cur = cur.next.next;
  }
  // 3.拆分两链表
  cur = head.next;
  let pre = head, res = head.next;
  while(cur.next){
    pre.next = pre.next.next;
    cur.next = cur.next.next;
    pre = pre.next;
    cur = cur.next;
  }
  pre.next = null; // 处理旧链表表尾
  return res;	// 返回新链表头节点
}
```

- 时间复杂度O(N)：三轮遍历链表
- 空间复杂度O(1)：引用变量使用常数大小的空间。

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

