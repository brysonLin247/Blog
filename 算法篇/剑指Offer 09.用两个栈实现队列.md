## 用两个栈实现队列

> 剑指Offer 09.用两个栈实现队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数`appendTail`和`deleteHead`，分别完成在队列尾部插入整数和在队列头部删除整数的功能。（若队列中没有元素，`deleteHead`操作返回 -1）

示例1:

```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```

示例2:

```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

提示：

- 1 <= values <= 10000
- 最多会对 appendTail、deleteHead 进行 10000 次调用

## 题解

队列：先入先出   栈：后入先出

根据题意，两个栈分工不同，一个为入队栈，一个为出队栈，各自负责入队和出队。

入队操作，直接压入入队栈即可，出队操作需要先检查出队栈有无数据，若无，需要从入队栈出栈导入到出队栈后再出栈即可。

```javascript
var CQueue = function(){
  this.stack1 = [] // 入队栈
  this.stack2 = [] // 出队栈
};
/** 
 * @param {number} value
 * @return {void}
 */
CQueue.prototype.appendTail = function(value){
  this.stack1.push(value)
};

/**
 * @return {number}
 */
CQueue.prototype.deleteHead = function(){
  if(this.stack2.length){
    return this.stack2.pop()
  }else{
   	while(this.stack1.length){
      this.stack2.push(this.stack1.pop())
    } 
    if(!this.stack2.length){
      return -1;
    }else{
      return this.stack2.pop()
    }
  }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * var obj = new CQueue()
 * obj.appendTail(value)
 * var param_2 = obj.deleteHead()
 */
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

