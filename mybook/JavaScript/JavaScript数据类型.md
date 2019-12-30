# JavaScript数据类型

搬运自：
[浅谈 javascript 六种数据类型以及特殊注意点 - 浅谈 JavaScript - 极客学院Wiki](http://wiki.jikexueyuan.com/project/brief-talk-js/on-six-data-type.html)
[JavaScript 深入了解基本类型和引用类型的值 - percy507的blog - SegmentFault 思否](https://segmentfault.com/a/1190000006752076)

Js中七种数据类型：
Undefined、Null、String、Number、Boolean、Object、Symbol（ES6新加）
Symbol表示独一无二的值。

## 基本类型（基本数据类型、原始数据类型等意）

 6 种基本数据类型：Undefined、Null、Boolean、Number、String、Symbol

1，基本类型的值是不可变的
2，基本类型的比较是它们的值的比较
3，基本类型的变量是存放在栈内存（Stack）里的

## 引用类型

引用类型，统称为 Object 类型。细分的话，有：Object 类型、Array 类型、Date 类型、RegExp 类型、Function 类型 等

1，引用类型的值是可变的
2，引用类型的比较是引用的比较
3，引用类型的值是保存在堆内存（Heap）中的对象（Object）
与其他编程语言不同，JavaScript 不能直接操作对象的内存空间（堆内存）。

## instanceof：

用来判断某个构造函数的 prototype 属性所指向的对象是否存在于另外一个要检测对象的原型链上

简单说就是判断一个引用类型的变量具体是不是某种类型的对象

```
console.log({} instanceof Object); // true
console.log([] instanceof Object); // true
console.log([] instanceof Array);  // true
console.log({} instanceof Array);  // false

(/aa/g) instanceof RegExp           // true
(function(){}) instanceof Function  // true
```

## typeof

注意
1，typeof是操作符，不是方法
2，对null取typeof是object（空的对象引用），对函数取typeof是function

```
>console.info(typeof null);
 object

>function f(){
console.log('helo');
}
console.log(typeof f);

 function
```

## 为各种数据类型的对象变量设置初始值

注意：如果Object类型的对象变量开始不知道赋值什么，不要用var obj={},最好是var obj = null;

```
var obj = null;
obj = {a:'b'};

var a = ''; // 字符串型
var b = 0;  // Number类型
```

## undefined和null的区别和注意点

1，如果使用“==”比较，它们相等
2，如何区分（核心是比较数据类型）
（1）typeof
（2）“===” 强比较，比较值和类型
3，js区分大小写，NULL，Null 等都不是null

```
console.log(undefined == null); // true
console.log(typeof undefined == typeof null); // false
console.log(undefined === null); // false
console.log(typeof NULL);  // undefined
```

## Boolean注意点

1，“==”的比较，true和1（无论是1还是‘1’）是相等的，false和0（或‘0’）是相等的，这是因为内部实现了隐式转换。true转化为1，false为0。但是“===”不等。
2，显示转换为Boolean，使用 Boolean() 方法显示转换，需要注意的是各种数据类型，什么时候转换成 true 什么时候转换成 false
1）String 类型，只要不是 空字符串 都会 转换成 true
2）Number 类型，只要不是 0 ，即使是 负数，都会转换成 true
3）Object 类型，只要 不是 null 类型，都会转换成 true
4）Undefined 类型，都会转换成 false
有意思的一点是‘0’==false 是true，而Boolean(‘0’) == true

```
console.log('1'==true); // true
console.log(1==true);   // true
console.log(0==false);  // true
console.log(false =='0'); // true

console.log(Boolean(-1)); // true
console.log(Boolean('0'));  // true
console.log(Boolean(null)); // false
console.log(Boolean({}));   // true
console.log(Boolean(undefined)); // false

```

## Number注意点

1，float类型 不能精确运算
2，支持 科学计数法
3，NaN （Not a Number)
（1）var a = 0/0, 在js里不报错，返回NaN `console.log(0/0); // NaN`
（2）可以通过Number.Nan获取
（3）NaN 和 任何 对象做运算都会返回 NaN
（4）isNaN() 判断是不是 NaN

```
console.log(isNaN(NaN)); // true
console.log(isNaN(true));// false  boolean转换为数字true 1
console.log(isNaN('a'));  // true
console.log(isNaN('2'));  // false 数字字符串直接转换成数字
```

（5）isNaN()内部执行原理：同样适用于对象。实现原理：首先调用对象的 valueOf()方法，如果能转换成数字就直接做判断；如果不能就再调用 toString()方法，然后测试返回值。

```
var b = {
toString: function(){ // 重写toString()方法
return '123';
}};
console.log(isNaN(b)); // false
console.log(b);  // {toString: ƒ}
alert(b);  // 123  alert()内部也是先调用了对象的valueOf()，然后调用toString()方法
```

（6）将其他数据类型转换成 Number 类型
含有三个函数：Number(): 可以针对所有的数据类型进行转换；parseInt()和 parseFloat()只针对 字符串进行转化。

```
console.log(Number(true)); // 1  
console.log(Number(null)); // 0
console.log(Number('0222'));// 222
console.log(Number(undefined)); // NaN
```

Number()内部实现的原理：同 isNaN()也是先调用 valueOf()然后调用toString()。所以可想而知，性能是比较差的。所以只要是被转型的对象是字符串的话，就调用parseInt()或者parseFloat()，因为他们内部不需要对类型做判断。
parseInt()和 parseFloat()调用注意：从第一个为数字的字符开始一直到第一个不为数字的字符的前一个数字的这部分字符串转换成数字
当 parseInt()里面的参数是 float 类型的那么只取得数字的整数部分

```
console.log(parseInt('ab234'));  // NaN
console.log(parseInt('323rer32')); // 323

console.log(parseInt(56.12)); // 56
```

## String 类型

1）（ 重要 ）在 ECMAScript 中 字符串有不变性：字符串创建之后就不会再改变。
要改变一个已经被赋值的字符串变量，首先要先销毁变量中字符串，然后再用一个包含新值的字符串填充变量。

```
var d='hello';  
d=d+'shit';// 执行过程：先将'hello'赋值一份，然后将d中的字符串清空，将字符串'hello'和'shit'进行拼接，然后赋值给d变量。（所以字符串的值一旦被 创建之后就不会改变)
```

2）toString()方法将其他数据类型转换成 String 类型。但是如果对 null 或 undefined 进行操作的话就会报错。
3）String()方法同样能实现 toString()的效果，但是可以对null和undefined进行操作。
内部原理：先调用toString()，如果可以转换成字符串，就将结果直接返回。否则，再进行判断是null还是undefined，然后返回‘null' 或 ‘undefined'
总结：如果知道变量不可能是null或undefined，就使用toString()，性能比 String()高，因为String()内部还要做判断，所以有损性能。

（完）