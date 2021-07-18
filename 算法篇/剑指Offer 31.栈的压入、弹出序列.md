## 栈的压入、弹出序列

> 剑指Offer 31.栈的压入、弹出序列
>
> 难度：中等

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列{1,2,3,4,5}是某栈的压栈序列，序列{4,5,3,2,1}是该压栈序列对应的一个弹出序列，但{4,3,5,1,2}就不可能是该压栈序列的弹出序列。

示例1:

```
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

示例2：

```
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```

提示：

1. 0 <= pushed.length == popped.length <= 1000
2. 0 <= pushed[i]，popped[i] < 1000
3. pushed是popped的排列

## 题解

### 辅助栈

构造一个辅助栈来stack模拟压栈序列pushed的出栈，主要步骤如下：

1. 入栈：按照压栈序列pushed的顺序入栈。
2. 出栈：每次出栈后，需要循环判断stack的栈顶是否等于弹出序列popped的当前元素，将符合弹出序列顺序popped的栈顶全部弹出。

```javascript
/**
 * @param {number[]} pushed
 * @param {number[]} popped
 * @return {boolean}
 */
var validateStackSequences = function(pushed,popped){
  // 辅助栈
  const stack = [];
  // 指向popped当前的下标
  let index = 0; 
  // 把pushed的元素一个个入栈
  for(let i = 0,len = pushed.length - 1;i <= len;i++){
    stack.push(pushed[i]);
    // 把入栈的当前元素和popped当前指向的元素进行对比
    // 相等的话就把辅助栈出栈
    // popped下标往右移动
    while(stack.length > 0 && stack[stack.length - 1] === popped[index]){
      stack.pop();
      index++;
    }
  }
  return !stack.length;
}
```

****

坚持每日一练！前端小萌新一枚，希望能点个`赞`+`在看`哇～

