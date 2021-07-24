## 数据流中的中位数

> 剑指Offer 41.数据流中的中位数
>
> 难度：困难
>
> 题目：https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4]的中位数是$3$

[2,3]的中位数是$(2+3)/2 = 2.5$

设计一种支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中
- double findMedian() - 返回目前所有元素的中位数。

示例1：

```
输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]
```

示例2：

```
输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]
```

限制：最多会对addNum、findMedian进行50000次调用

## 题解

### 法一 直接法

对所有元素先排序，计算出中位数后才取出中位数。

```javascript
/**
 * initialize your data structure here.
 */
var MedianFinder = function () {
  this.result = [];
};

/** 
 * @param {number} num
 * @return {void}
 */
MedianFinder.prototype.addNum = function (num) {
  this.result.push(num);
};

/**
 * @return {number}
 */
MedianFinder.prototype.findMedian = function () {
  const len = this.result.length;
  if (!len) return null;
  this.result.sort((a, b) => a - b);
  const mid = Math.floor((len - 1) / 2); // 向下取整
  if (len % 2) {
    return this.result[mid];
  }
  return (this.result[mid] + this.result[mid + 1]) / 2;
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * var obj = new MedianFinder()
 * obj.addNum(num)
 * var param_2 = obj.findMedian()
 */
```

- 时间复杂度：O($NlogN$)，sort使用快排
- 空间复杂度：O($NlogN$)

### 法二 二分查找

每次添加元素前都保证数组有序，利用二分查找到方式找到正确位置并将其新元素插入。

```javascript
var MedianFinder = function () {
  this.result = [];
};

MedianFinder.prototype.addNum = function (num) {
  const len = this.result.length;
  if (!len) {
    this.result.push(num);
    return;
  }

  let l = 0,
    r = len - 1;
  while (l <= r) {
    let mid = Math.floor((l + r) / 2);
    if (this.result[mid] === num) {
      this.result.splice(mid, 0, num);
      return;
    } else if (this.result[mid] > num) {
      r = mid - 1;
    } else {
      l = mid + 1;
    }
  }
  this.result.splice(l, 0, num);
};

MedianFinder.prototype.findMedian = function () {
  const len = this.result.length;
  if (!len) return null;
  const mid = Math.floor((len - 1) / 2);
  if (len % 2) {
    return this.result[mid];
  }
  return (this.result[mid] + this.result[mid + 1]) / 2;
};
```

- 时间复杂度：O($N$)，二分查找需要O($logN$)，移动需要O($N$)，故为O($N$)
- 空间复杂度：O($N$)

### 法三 优先队列（堆）

用大小堆，最大堆存放数据流中较小的一半元素，最小堆存放数据流中较大堆一元素，需要保持`两个堆的大小相等`或者`最大堆 = 最小堆 + 1`。（奇数与偶数）

当调用findMedian时，中位数即为**最大堆的堆顶元素**或者（**最大堆的堆顶 + 最小堆的堆顶）/ 2**。（奇数与偶数）

保持相等的做法：

- 先让num放入最大堆maxHeap，取出maxHeap的堆顶元素，放入最小堆minHeap。
- 若最大堆的大小 < 最小堆的大小，取出minHeap的堆顶元素，放入maxHeap。

JavaScript没有堆的函数，需要自己手写，以下是二叉堆的实现代码：

```javascript
const defaultCmp = (x, y) => x > y;// 用于控制判断，从而实现最大堆与最小堆的切换
const swap = (arr, i, j) => ([arr[i], arr[j]] = [arr[j], arr[i]]);// 用于交换两个节点
class Heap {
	constructor(cmp = defaultCmp){
    this.container = []; // 用于存放数据
    this.cmp = cmp;
  }
  
  push_heap(data) { // 用于添加节点
    const { container, cmp } = this;
    container.push(data); // 先将值插入到二叉堆的最后一位
    let index = container.length - 1;// data值的坐标
    while(index){
      let parent = Math.floor((index - 1) / 2); // data的父节点
      if(!cmp(container[index], container[parent])){ // 以最大堆为例，当子节点小于父节点，直接返回。
        return;
      }
      swap(container, index, parent);// 当子节点大于父节点，交换
      index = parent;// 继续递归对比
    }
  }
  
  pop_heap() { // 用于弹出节点，弹出堆顶元素后，需要调整堆，并返回堆顶堆值。
    const { container, cmp } = this;
    if(!container.length){
      return null;
    }
    swap(container, 0, container.length - 1);//先讲堆顶元素与尾部元素交换
    const res = container.pop();// 再弹出尾部元素
    const length = container.length;
    let index = 0, exchange = index * 2 + 1;// exchange为左节点下标
    
    while(exchange < length) {
      let right = index * 2 + 2;
      // 以最大堆的情况，如果有右节点，并且右节点的值大于左节点的值
      if(right < length && cmp(container[right], container[exchange])) {
        exchange = right;
      }
      if(!cmp(container[exchange], container[index])) {
        break;
      }
      swap(container, exchange, index);
      index = exchange;
      exchange = index * 2 + 1;
    }
    return res;
  }
  
  top() { // 输出堆顶值
    if(this.container.length) return this.container[0];
    return null;
  }
}                                                 
```

![优先队列入队操作](/Users/brysonlin/Library/Application Support/typora-user-images/image-20210724110141834.png)

![优先队列出队操作](/Users/brysonlin/Library/Application Support/typora-user-images/image-20210724110313038.png)





有了堆的实现后，接下来来看本题：

```javascript
var MedianFinder = function() {
	this.maxHeap = new Heap();
  this.minHeap = new Heap((x, y) => x < y);
};

MedianFinder.prototype.addNum = function(num) {
	this.maxHeap.push_heap(num);
  this.minHeap.push_heap(this.maxHeap.pop_heap());
  
  if(this.maxHeap.container.length < this.minHeap.container.length){
    this.maxHeap.push_heap(this.minHeap.pop_heap());
  }
};

MedianFinder.prototype.findMedian = function() {
	return this.maxHeap.container.length > this.minHeap.container.length ? this.maxHeap.top() : (this.maxHeap.top() + this.minHeap.top()) / 2;
};
```

- 时间复杂度：O($logN$)
- 空间复杂度：O($N$)

参考链接：https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/solution/tu-xie-zheng-li-bao-li-fa-er-fen-cha-zhao-shou-don/

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

