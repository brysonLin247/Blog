> 剑指Offer 05.替换空格

## 替换空格

请实现一个函数，把字符串 s 中的每个空格替换成“%20”。

示例：

```
输入：s = “We are happy.”
输出：“We%20are%20happy.”
```

限制：0 <= s的长度 <= 10000

## 题解

解法非常多，利用很多原生api就能解决，如

- replace / replaceAll
- encodeURIComponent
- split / join

或者直接暴力法等，但这些都不是考察等重点。

### 双指针法

时间复杂度O(n) 空间复杂度O(1)

因为JS中字符串无法被修改，一旦给字符串变量重新赋值，就要花费时间和空间去重新新建一个字符串，从而增加了复杂度。

所以我们要采用数组来进行操作，流程如下：

1. 将字符串转换为数组，然后统计其中的空格数量。
2. 根据空格数量和原有字符串有效字符长度，计算出刚好存放替换后的字符长度的数组。
3. 创建两个指针，一个指数组末尾，一个指字符串有效位的末尾，实现原地修改。

其中，数组遍历是从后往前的遍历，避免从前往后，造成字符被修改导致错误。

```javascript
var replaceSpace = function(s){
  s = s.split(""); // 用于把字符串转为字符串数组
  let oldLen = s.length;
  let spaceCount = 0;
  for(let i = 0; i < oldLen; i++){
    if(s[i] === ' ') spaceCount++;
  }
  s.length += spaceCount * 2;
  for(let i = oldLen - 1, j = s.length - 1; i >= 0; i--, j--){
    if(s[i] !== ' ') s[j] = s[i];
    else {
      s[j-2] = '%';
      s[j-1] = '2';
      s[j] = '0';
      j -= 2;
    }
  }
  return s.join(''); // 用于把数组中的所有元素放入一个字符串
}
```

结果：

![image-20210623135829557](/Users/brysonlin/Library/Application Support/typora-user-images/image-20210623135829557.png)

### 法二 遍历添加

新建一个字符串去操作，时间复杂度和空间复杂度均为O(n)，这里就不展开讨论了。

