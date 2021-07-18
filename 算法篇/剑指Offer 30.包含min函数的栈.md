## 包含min函数的栈

> 剑指Offer 30.包含min函数的栈
>
> 难度：简单

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的min函数在栈中，调用min、push及pop的时间复杂度都是O(1)。

示例：

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```

提示：各函数的调用总次数不超过20000次

## 题解

据题意，需要在O(1)时间找到最小值，这意味着我们不能在寻找最小值的时候，再做排序，查找等操作去获取。

### 法一 辅助栈法

可以采用空间换时间的原则，创建两个栈，一个栈是主栈stack，另一个是辅助栈minStack，用于存放对应主栈不同时期的最小值。

```javascript
/**
 * initialize your data structure here.
 */
var MinStack = function() {
	this.stack = [];
  this.minStack = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
	this.stack.push(x);
  // 保持主栈和辅助栈同步，在辅助栈中，存放着每一位主栈元素对应的最小值
  this.minStack.push(Math.min(this.minStack[this.minStack.length - 1], x));
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
	this.stack.pop();
  this.minStack.pop();
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
	return this.stack[this.stack.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.min = function() {
	return this.minStack[this.minStack.length - 1];
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.min()
 */
```

### 法二 迭代

其实挺类似辅助栈法的，迭代法是通过在栈里面存储进一个对象「x，min」的方式，在每一次对栈往里存或者弹出都要保持「x，min」同步，每一个对象中都存放着对应的最小值。

```javascript
var MinStack = function() {
    this.stack = [];
};

MinStack.prototype.push = function(x) {
    this.stack.push({
        x,min: this.stack.length ? Math.min(this.head().min,x) : x
    });
};

MinStack.prototype.pop = function() {
    this.stack.pop();
};

MinStack.prototype.top = function() {
    return this.head().x;
};

MinStack.prototype.min = function() {
    return this.head().min;
};
// 当前栈的最后一项 即栈顶头 head
MinStack.prototype.head = function(){
    return this.stack[this.stack.length - 1];
}

```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

