# this的用法

---

当一个函数被调用时，会建立一个活动记录，也称为执行上下文。这个记录包含函数是从何处被调用的，**函数是如何被调用的**，被传递了什么参数等信息。这个记录的属性之一，就是在函数执行期间将被使用的 this 引用。

注：this 关键字是 JavaScript 中最复杂的机制之一。this 被自动定义在所有函数的作用域中。

---

> this的特点

 函数的调用方式决定了 this 的值，**运行期间**绑定
 每次函数被调用时 this 的值也可能会不同
 this 不能被赋值
 **this 不进行作用域传递**

---

> this的作用

1. 复用代码
2. 提供了一个更加优雅而灵便的方案，传递一个隐式的对象引用让代码变得更加简洁

---

> 绑定规则

默认绑定
隐式绑定
显示绑定
new绑定

---

> 默认绑定

**独立函数调用。可以把这条规则看作是无法应用其他规则时的默认规则。**
使用非严格模式，this 会绑定到全局对象 window 上。
使用严格模式， this 会绑定到 undefined。
```
//非严格模式
var a = 2;

function foo() {
    console.log(this);
    console.log(this.a);
}
foo();
```
```
//严格模式
var a = 2;

function foo() {
    "use strict"
    console.log(this);
    console.log(this.a);
}
foo();
```

**函数嵌套时，需注意 this 不进行作用域传递**。

---

> 隐式绑定

**调用位置是否有上下文对象。**
函数作为对象的方法调用时，函数上下文指向这个对象。
```
//例1
var a = 100;
var obj = {
    a: 300,
    fun: function () {
        console.log(this.a);
    }
};
obj.fun(); //300
```
```
//例2
var a = 100;
var obj = {
    a: 300,
    fun: function () {
        console.log(this.a);
    }
};
var fun = obj.fun;
fun(); //100
obj.fun(); //300
```

**数组中存放函数，被数组索引调用。this上下文指向这个数组。**
```
function fun() {
    console.log(this.length);
}
var length = 10;
var arr = [100, 200, fun];
fun(); //10
arr[2](); //3
```

**隐式绑定 —— 就近原则**
```
//例1
function foo() {
    console.log(this.a);
}
var obj2 = {
    a: 42,
    foo: foo
};
var obj1 = {
    a: 2,
    obj2: obj2
};

obj1.obj2.foo(); // 42
```

```
//例2
var a = 0;

function fun() {
    console.log(this.a)
}
var obj = {
    a: 1,
    b: 2,
    c: [{
        a: 3,
        b: 4,
        c: fun
    }]
};
obj.c[0].c(); //3
// 当一个函数有被指定上级对象的时候，this仅指向最靠近的上级（父）对象。如 foo.fn.o() , o里面的this指向fn
//函数 最终调用者 obj.c[0]是一个对象:
// {
//    a:3,
//    b:4,
//    c:fun
// }
```

---

> 显示绑定

**使用 call()、apply()、bind() 方法显示改变 this 的指向为指定对象。**

---

> new绑定

**new 来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。**
创建（或者说构造）一个全新的对象。
这个新对象会被执行 [[ 原型 ]] 连接。
这个新对象会绑定到函数调用的 this。
如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回 this (新对象)。
```
function Person(name, age) {
    this.name = name;
    this.age = age;
}
var pson = new Person("Lily", 20);


//new 作用示意代码
function New(func,name,age) {
    //第一步：创建空对象
    var obj = {};
    //第二步：执行 [[ 原型 ]] 连接
    obj.__proto__ = func.prototype;
    //第三步：新对象会绑定到函数调用的 this
    func.apply(obj,Array.prototype.slice.call(arguments,1));
    //this.name = name;//obj.name = name;
    //this.age = age;//obj.age = age;
    //第四步：返回 this (新对象)
    return obj; //return this;
}
```

---

> 绑定优先级

**new 绑定 > 显式绑定 > 隐式绑定 > 默认绑定**
**判断 this 的顺序**
函数是否在 new 中调用？如果是的话 this 绑定的是新创建的对象。
函数是否通过 call、apply、bind调用？如果是的话 this 绑定的是指定的对象。
函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上下文对象。
如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 undefined，否则绑定到全局对象。
