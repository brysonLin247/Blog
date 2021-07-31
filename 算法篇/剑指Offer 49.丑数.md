## 丑数

> 剑指Offer 49.丑数
>
> 难度：中等
>
> 题目：https://leetcode-cn.com/problems/chou-shu-lcof/

我们把只包含因子2、3和5的数称作丑数（Ugly Number）。求按从小到大的顺序的第n个丑数。

示例：

```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```

说明：

1. 1是丑数
2. n不超过1690

## 题解

### 法一 动态规划

由题意知，只包含2、3和5的数称作丑数，所以下一个丑数一定是前面的某个丑数乘2、3或5所得。因此，准备三个指针p2、p3、p5分别指向乘2、3和5。在每次循环时，取`2*res[p2]`、`3*res[p3]`和 `5*res[p5]`结果最小的数进行排序，并将对应的指针向前移动。当循环结束时，返回数组res。

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var nthUglyNumber = function (n) {
  const res = new Array(n);
  res[0] = 1; // 1是丑数
  let p2 = 0,
    p3 = 0,
    p5 = 0;

  for (let i = 1; i < n; i++) {
    res[i] = Math.min(2 * res[p2], 3 * res[p3], 5 * res[p5]);
    if (res[i] === 2 * res[p2]) {
      p2++;
    }
    if (res[i] === 3 * res[p3]) {
      p3++;
    }
    if (res[i] === 5 * res[p5]) {
      p5++;
    }
  }
  return res[n - 1];
};
```

- 时间复杂度：O($N$)
- 空间复杂度：O($N$)

### 法二 最小堆

设置最小堆heap，使用哈希集合map来记录丑数是否出现过，用于去重。步骤如下：

- 初始时堆为空，首先将最小的丑数1加入堆。

- 每次取出堆顶元素x，则x为堆中最小的丑数，同时需要将2x，3x，5x加入堆，加入之前需要检查结果是否出现过，如果没出现过，则放入堆中并更新map。
- 返回结果中最后一个数

javascript中没有堆的函数，需要自己手写，以下是最小堆的实现代码：

```javascript
class Minheap {
  constructor(){
    this.container = []; // 用于存放数据
  }
  
  push_heap(data) { // 用于添加节点
    const { container } = this;
    container.push(data); // 先将值插入到二叉堆的最后一位
    let index = container.length - 1; // data值的下标
    while(index) {
      let parent = Math.floor((index - 1) / 2); // data的父节点
      if(container[index] > container[parent]){ // 当子节点大于父节点，直接返回
        return;
      }
      // 当子节点小于父节点，交换
      [container[index],container[parent]] = [container[parent],container[index]];
    	index = parent; // 继续递归对比
    }
  }
  
  pop_heap() { // 用于弹出节点，弹出堆顶元素后，需要调整堆，并返回堆顶值
    const { container } = this;
    if(!container) return null;
    [container[0],container[container.length - 1]] = [container[container.length - 1],container[0]]; // 先将堆顶元素与尾部元素交换
    const res = container.pop(); // 再弹出尾部元素
    let index = 0, exchange = index * 2 + 1; // exchange为左节点下标
    const len = container.length;
    
    while(exchange < len) {
      let right = index * 2 + 2;
      // 如果有右节点，并且右节点的值小于左节点的值
      if(right < len && container[right] < container[exchange]){
        exchange = right;
      }
      if(container[exchange] > container[index]){
        break;
      }
      [container[exchange],container[index]] = [container[index],container[exchange]];
      index = exchange;
      exchange = index * 2 + 1;
    }
    return res;
  }
}
```

题解：

```javascript
var nthUglyNumber = function (n) {
  const heap = new Minheap();
  const res = new Array(n);
  const map = new Map();
  const nums = [2, 3, 5];

  heap.push_heap(1);
  map.set(1, true);
  for (let i = 0; i < n; i++) {
    res[i] = heap.pop_heap();

    for (const num of nums) {
      let tmp = res[i] * num;
      if (!map.get(tmp)) {
        heap.push_heap(tmp);
        map.set(tmp, true);
      }
    }

  }
  return res[n - 1];
}
```

- 时间复杂度：O($NlogN$)，得到第n个丑数需要n次循环，最小堆需要O($logN$)
- 空间复杂度：O($N$)

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

