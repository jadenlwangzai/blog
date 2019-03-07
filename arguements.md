## 问题

```javascript
var length = 10;
function fn(){
    alert(this.length)
}
var obj = {
    length: 5,
    method: function(fn) {
    arguments[0]()
     }
}
obj.method(fn);//1
```

这段代码中的`arguments[0]()`是第一个参数？带一对小括号是什么意思？

## 理解

我们可以先从最后调用`obj.method(fn)`开始理解。

1.`obj`是对象，`method（）`是`obj`的方法，`fn`是`method()`的参数，`fn`是函数的名, 他引用对应的函数。`arguments`是javascript的一个内置对象

>An Array-like object corresponding to the arguments passed to a function.<br>The arguments object is a local variable available within all functions; arguments as a property of Function can no longer be used.
Description:You can refer to a function‘s arguments within the function by using the arguments object. This object contains an entry for each argument passed to the function, the first entry’s index starting at 0. 

2.`arguments`是用来取得`method(fn)`函数的参数数组,在这里也就是fn，即`arguments[0]===fn`。所以`arguments[0]()`就等于`fn()`

妈蛋，是不是到这里要开始风中凌乱了，`this.length`究竟是指向那个对象呢?
可以这样理解：

```javascript
var arguments={
     fn:function(){
     alert(this.length)
  },
```
或者更可以这样理解：

```javascript
arguments = {
 0: fn, //也就是 functon() {alert(this.length)} 
 1: 第二个参数, //没有 
 2: 第三个参数, //没有
 ..., 
 length: 1 //只有一个参数
}
```

最后，这个1就是arguments.length，也就是本函数参数的个数。
