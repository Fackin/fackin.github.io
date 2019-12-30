# apply()、call()和bind()

所有函数都定义了`apply()`和`call()`。

## apply()和call()的区别

传参格式不同

`apply()`两个参数，第一个是函数上下文的对象，后一个是作为函数参数所组成的数组

```javascript
var obj = {
  name: 'XiaoMing'
}
function func(age, work) {
  console.log(this.name + ' is a ' + work + '(' + age + ').');
}
func.apply(obj, ['18', 'student']);
```

`call()`第一个参数也是函数上下文的对象，后面是作为函数的参数列表

```javascript
var obj = {
  name: 'XiaoMing'
}
function func(age, work) {
  console.log(this.name + ' is a ' + work + '(' + age + ').');
}
func.call(obj, '18', 'student');
```



## `apply()`和`call()`的用法

### 1.改变this的指向

```javascript
var obj = {
  name: 'XiaoMing'
}
function func() {
  console.log(this.name);
}
func.call(obj);

//相当于
function func() {
  console.log(obj.name);
}
```

### 2.继承别的对象的方法

```javascript
var Person1  = function () {
    this.name = 'linxin';
    this.getname = function (){
      console.log('oldname');
    }
}
var Person2 = function () {
  this.name = 'dddd';
  this.age = 13;
//     Person1.call(this);
    this.getname = function () {
        console.log(this.name, this.age);
    }
    Person1.call(this);
}
var person = new Person2();
person.getname();       // oldname
```

### 3.调用函数

`apply()`和`call()`都是立即执行的。所以可以用作调用函数

```javascript
function func () {
  console.log('aaa');
}
func.call();
```

### 4.`apply()`作为参数转换

```javascript
var max = Math.max(1, 4, 8, 9, 0);


var arr = [1, 4, 8, 9, 0];
var max1 = Math.max.apply(null, arr);
```



## call()和bind()的区别

在 EcmaScript5 中扩展了叫 bind 的方法，在低版本的 IE 中不兼容。它和 call 很相似，接受的参数有两部分，第一个参数是是作为函数上下文的对象，第二部分参数是个列表，可以接受多个参数。
它们之间的区别有以下两点。

### 1.bind()的返回值是函数

```javascript
function func () {
  console.log('aaa');
}
var func1 = func.bind();
func1();
```

### 2.参数的使用

```javascript
function func(a,b,c) {
  console.log(a,b,c);
}
var func1 = func.bind(null, 'XX');

func('A','B','C'); //'A' 'B' 'C'
func1('A','B','C'); //'XX' 'A' 'B'
func1('S','B');     // 'XX' 'S' 'B'
func.call(null, 'YY'); //'YY' undefined undefined

```



在低版本浏览器没有 bind 方法，我们也可以自己实现一个。

```javascript
if (!Function.prototype.bind) {
  Function.prototype.bind = function () {
    var self = this,                        // 保存原函数
    context = [].shift.call(arguments), // 保存需要绑定的this上下文
    args = [].slice.call(arguments);    // 剩余的参数转为数组
    return function () {                    // 返回一个新函数
    	self.apply(context,[].concat.call(args, [].slice.call(arguments)));
    }
  }
}
```

https://github.com/lin-xin/blog/issues/7

https://www.cnblogs.com/coco1s/p/4833199.html