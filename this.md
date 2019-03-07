## 问题

问题源自于[厉瑶blog](www.liyao.mame)上面的一个题目：

```javascript
var info = "out of ob";
var ob = {
 info: "in ob",
 msg: function(){console.log(this.info);
 }
};
ob.msg();//in ob
var outmsg = ob.msg;
outmsg();//out of ob
var bindmsg = outmsg.bind(ob);
bindmsg();//in ob
```

1. 为什么调用outmsg()控制台输出的是“out of ob”？
2. `bindmsg = outmsg.bind(ob);`怎么理解绑定到ob时候有输出 in ob?

## 理解

先需要解释的就是这段javascript代码不是在严格模式下的，基于这样的前提，无论是执行`ob.msg()`还是`outmsg()`,当控制权进入函数或者方法上下文时，都会遵循以下几步.

1. 调用者提供一个 thisArg 
2. 如果 thisArg 为 null 或 undefined,则令 this 引向全局对象（浏览器下即为 window）
3. 否则 ，如果thisArg 不是一个对象，则根据 Annotated ES5 进行转换 ，并使 this 指向 转换结果
4. 如果 thisArg 是一个对象，则使 this 指向 thisArg

   * `ob.msg()`：此时作为`ob`的一个方法，`this`自然是`ob`，这样一来就是`function（）{console.log(ob.info)}//in ob;`

   * `var outmsg=ob.msg`：通过`ob.msg.constructor === Function;// true`可以知道 `outmsg`就是一个函数，不是一个对象的方法，所以`ob.msg()`此时作为一个全局函数，this指示的就是window对象，即`window.info`,所以输出为`out of ob`

   * `var bindmsg = outmsg.bind(ob);`[bind方法](https://developer.mozilla.org/zh-CN/docs/JavaScript/Reference/Global_Objects/Function/bind),bind()称为绑定函数,是Function对象的一个属性。this是动态的，bind方法就是不让this乱动，绑定作用域。在这里把`outmsg`里面的所有`this`都指向了`ob`, 最后的`this.info`就是`ob.info`,输出in ob。

## 实践

```javascript
var info = "out of ob";
var ob = {
 info: "in ob",
 msg:function(){console.log(this.info);
   }
};
var aaaa = {info: "bbbb"}
ob.msg();//in ob
var outmsg = ob.msg;
outmsg();//out of ob
var bindmsg = outmsg.bind(aaaa);
bindmsg();//bbbb
```