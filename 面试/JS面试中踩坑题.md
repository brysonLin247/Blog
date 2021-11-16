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

### 请讲讲该代码执行上下文具体执行过程

```javascript
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f();
}
checkscope();
```

执行过程如下：

1. 执行全局代码，创建全局执行上下文，全局上下文被压入执行上下文栈

```javascript
ECStack = [
	globalContext
]
```

2. 全局上下文初始化

```javascript
globalContext = {
  VO: [global],
  Scope: [globalContext.VO],
  this: globalContext.VO
}
```

3. 初始化的同时，创建checkscope函数，保存作用域到内部属性[[scope]]

```javascript
checkscope.[[scope]] = [
  globalContent.VO
]
```

4. 执行checkscope函数，创建checkscope函数执行上下文并压入执行上下文栈

```javascript
ECStack = [
  checkscopeContext,
  globalContext
]
```

5. checkscope函数执行上下文初始化：

- 复制函数[[scope]]属性创建作用域链

```javascript
checkscopeContext = {
	Scope: checkscope.[[scope]]
}
```

- 用argument创建活动对象，给变量对象添加初始属性值（形参、函数声明、变量声明等）。

```javascript
checkscopeContext = {
	AO:{
    arguments:{
      length:0
    },
    scope:undefined,
    f: reference to function f(){}
  }
}
```

- 将活动对象AO压入checkscope作用域链顶端

```javascript
checkscopeContext = {
	AO:{
    arguments:{
      length:0
    },
    scope:undefined,
    f: reference to function f(){}
  },
  Scope: [AO,globalContext.VO],
  this:undefined
}
```

6. 执行f函数，创建f函数执行上下文，f函数执行上下文被压入执行上下文栈

```javascript
ECStack = [
  fContext,
  checkscopeContext,
  globalContext
];
```

7. f函数执行上下文初始化，与第5一样

```javascript
fContext = {
  AO: {
    arguments: {
      length: 0
    }
  },
  Scope: [AO, checkscopeContext.AO, globalContext.VO],
  this: undefined
}
```

8. f函数执行，沿着作用域链查找scope值并返回，执行阶段，会将执行上下文中有关参数赋值
9. f函数执行完毕，f函数上下文从执行上下文栈中弹出

```javascript
ECStack = [
  checkscopeContext,
  globalContext
];
```

10. checkscope函数执行完毕，checkscope函数上下文从执行上下文栈中弹出

```javascript
ECStack = [
    globalContext
];
```

### 看题说输出结果

```javascript
var data = [];

for (var i = 0; i < 3; i++) {
  data[i] = function () {
    console.log(i);
  };
}

data[0]();
data[1]();
data[2]();
```

答案都是3，原因：

当执行到data[0]时，此时全局上下文的VO为：

```javascript
globalContext = {
  VO: {
    data: [...],
    i: 3
  }
}
```

当执行data[0]时，data[0]函数的作用域链为：

```javascript
data[0]Context = {
    Scope: [AO, globalContext.VO]
}
```

data[0]Context的AO并没有 i 值，所以会从globalContext.VO中查找，i 为3，所以打印的结果就是3。

现在改成闭包：

```javascript
var data = [];

for (var i = 0; i < 3; i++) {
  data[i] = (function (i) {
        return function(){
            console.log(i);
        }
  })(i);
}

data[0](); // 0
data[1](); // 1
data[2](); // 2
```

当执行到data[0]时，此时全局上下文的VO为：

```javascript
globalContext = {
  VO: {
    data: [...],
    i: 3
  }
}
```

当执行data[0]时，data[0]函数的作用域链为：

```javascript
data[0]Context = {
  Scope: [AO, 匿名函数Context.AO globalContext.VO]
}
```

匿名函数执行上下文的AO为：

```javascript
匿名函数Context = {
  AO: {
    arguments: {
      0: 0,
      length: 1
    },
    i: 0
  }
}
```

data[0]Context 的 AO 并没有i值，所以会沿着作用域链从匿名函数 Context.AO 中查找，这时候就会找 i 为 0，找到了就不会往 globalContext.VO 中查找了，即使 globalContext.VO 也有 i 的值(值为3)，所以打印的结果就是0。

### 看题说输出结果

```javascript
var value = 1;
function foo(v) {
    v = 2;
    console.log(v); //2
}
foo(value);
console.log(value) // 1
```

当传递 value 到函数 foo 中，相当于拷贝了一份 value，假设拷贝的这份叫 _value，函数中修改的都是 _value 的值，而不会影响原来的 value 值。

```javascript
var obj = {
    value: 1
};
function foo(o) {
    o.value = 2;
    console.log(o.value); //2
}
foo(obj);
console.log(obj.value) // 2
```

```javascript
var obj = {
    value: 1
};
function foo(o) {
    o = 2;
    console.log(o); //2
}
foo(obj);
console.log(obj.value) // 1
```

以上涉及到堆栈的问题。