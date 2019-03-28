# 第六次作业

---

> for，for-in，forEach 三种循环比较

1．for循环

```
var arr = ["a", "b", "c", "d"];
	for(var i = 0; i < arr.length; i++) {
	    console.log(arr[i]);
}
```

写法上不够简洁，但初学者经常使用

> for in 循环

**（1）索引为字符串类型**

```
var arr = ["a", "b", "c", "d"];
  for(var i in arr) {//这里的i代表的是key
    console.log(typeof i);//string，索引为字符串类型
    console.log(arr[i]);
}
```

**（2）无序列（不是按照书写顺序遍历输出）** 

```
var obj = {
            name: "hello",
            age: "18",
            11: 22
        }
        for(var i in obj) {
            console.log(i); //遍历对象的属性
            console.log(obj[i]); //遍历对象的属性值
}
```

**（3）可扩展属性也会遍历**

```
var arr = ["a", "b", "c", "d"];
        arr.name = "hello";//扩展属性
        for(var i in arr) { //这里的i代表的是key
            console.log(arr[i]);
            console.log(i)
}
```

> forEach循环

**forEach循环有两个缺点：**

a.不能中断循环(比如说用break语句或使用return语句）

b.低版本IE浏览器存在兼容性

```
var arr = ["a", "b", "c", "d"];
        arr.forEach(function(value) { //不能跳出循环操作
            console.log(value);
            break;//不起作用，会报错
    });
```

**Math String Array**

a.Math 是一个内置对象，它具有数学常数和函数的属性和方法。

b.与其它全局对象不同的是，Math 不是一个构造器。Math的所有属性和方法都是静态的。

c.Math不是一个函数对象。

类似以下定义：
```
var Math={
    		//属性和方法
    	}
    	function String(){
    		//this属性和方法
    	}
    	function Array(){
    		//this属性和方法
}
```

原型链：

     Math.__proto__ === Object.prototype//true
     String.__proto__ === Function.prototype//true
     String.__proto__.__proto__ === Object.prototype//true

---

> 方法封装