## 队列的最大值

> 剑指Offer 59 - II. 队列的最大值
>
> 难度：中等
>
> 题目：https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/

请定义一个队列并实现函数`max_value`得到队列里的最大值，要求函数`max_value`、`push_back`和`pop_front`的**均摊**时间复杂度都是O($1$)。

若队列为空，`pop_front`和`max_value`需要返回-1。

示例1：

```
输入: 
["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
[[],[1],[2],[],[],[]]
输出: [null,null,null,2,1,2]
```

示例2：

```
输入: 
["MaxQueue","pop_front","max_value"]
[[],[],[]]
输出: [null,-1,-1]
```

限制：

- 1 <= push_back, pop_front, max_value的总操作数 <= 10000
- 1 <= value <= 10^5

## 题解

均摊时间复杂度O(1)，因此所有节点只会入队出队一次。

### 法一 暴力法

出队和入队操作都是O($1$)，但是这里的求`max_value`使用了Math.max()，并不难实现**均摊**时间复杂度都是O($1$)的要求，不过AC能过，勉强可以看。

```javascript
var MaxQueue = function() {
    this.queue = [];
};

/**
 * @return {number}
 */
MaxQueue.prototype.max_value = function() {
    if(!this.queue.length) return -1;
    return Math.max(...this.queue);
};

/** 
 * @param {number} value
 * @return {void}
 */
MaxQueue.prototype.push_back = function(value) {
    this.queue.push(value);
};

/**
 * @return {number}
 */
MaxQueue.prototype.pop_front = function() {
    if(!this.queue.length) return -1;
    return this.queue.shift();
};

/**
 * Your MaxQueue object will be instantiated and called as such:
 * var obj = new MaxQueue()
 * var param_1 = obj.max_value()
 * obj.push_back(value)
 * var param_3 = obj.pop_front()
 */
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

### 法二 单调队列

本质上求队列最大值也是一个滑动窗口求最大值，利用一个单调递减队列来保存。

用一个单调队列deque来存储每个窗口的最大值，如果有新元素加入，则要判断新元素和deque的尾部元素的大小，要保持单调递减的特性。

队列queue在弹出元素前，如果弹出元素与单调队列deque的首元素一样的话，两者要同时弹出。

```javascript
var MaxQueue = function () {
  this.queue = [];
  this.deque = [];
};

MaxQueue.prototype.max_value = function () {
  return this.deque.length ? this.deque[0] : -1;
};

MaxQueue.prototype.push_back = function (value) {
  this.queue.push(value);
  while (this.deque.length && value > this.deque[this.deque.length - 1]) {
    this.deque.pop();
  }
  this.deque.push(value);
};

MaxQueue.prototype.pop_front = function () {
  if (!this.queue.length) return -1;
  if (this.queue[0] === this.deque[0]) {
    this.deque.shift();
  }
  return this.queue.shift();
};
```

- 时间复杂度：O($1$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

