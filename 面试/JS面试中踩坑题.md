## JS面试中踩坑题

### 看题输出结果

```javascript
function foo() {
    console.log(a);
    a = 1;
}

foo(); // ???

function bar() {
    a = 1;
    console.log(a);
}
bar(); // ???
```

第一段会报错`Uncaught ReferenceError: a is not defined。

第二段会打印1。

这是因为函数中的a并没有通过var关键字声明，所以不会被存放在活动对象（AO）中。第一段执行console.log的AO是：

```javascript
AO = {
    arguments: {
        length: 0
    }
}
```

### 看题输出结果

```javascript
console.log(foo);

function foo(){
    console.log("foo");
}

var foo = 1;
```

会打印函数，而不是undefined。

因为在进入执行上下文时，首先会处理函数声明，其次会处理变量声明，如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性。