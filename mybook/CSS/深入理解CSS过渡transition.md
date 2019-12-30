# 过渡transition



文章搬运：[深入理解CSS过渡transition - 小火柴的蓝色理想 - 博客园](https://www.cnblogs.com/xiaohuochai/p/5347930.html)
 [demos](https://fackin.github.io/example/transitionDemos/index.html) 

## 定义

过渡`transition`是一个复合属性，包括`transition-property`、`transition-duration`、`transition-timing-function`、`transition-delay`这四个子属性。通过这四个子属性的配合来完成一个完整的过渡效果。

```
transition-property: 过渡属性（默认all)
transition-duration: 过渡持续时间（默认值0s）
transition-timing-function: 过渡函数（默认值ease函数）
transition-delay: 过渡延迟时间（默认值0s）
```

**注意** IE9-不支持该属性，safari3.1-6、IOS3.2-6.1、android2.1-4.3需要添加-webkit-前缀；而其余高版本浏览器支持标准写法

```
transition: <transition-property> || <transition-duration> || <transition-timing-function> || <transition-delay>
```

四个属性中只有<transition-duration>是必填值且不能为0。其中，<transition-duration>和<transition-delay>都是时间。当两个时间同时出现时，第一个是<transition-duration>,第二个是<transition-delay>;当只有一个时间时，它是<transition-duration>，而<transition-delay>为默认值0。

**注意**transition的四个子属性之间不能用逗号分开，只能用空格。因为逗号隔开的是代表不同属性（transition属性支持多值）；而空格隔开代表不同属性的四个关于过渡的子属性。

## 过渡属性

### `transition-property`

```
值： none | all | <transition-property>[,<transition-property>]*
初始值： all
应用于： 所有元素
继承性： 无

none: 没有指定任何样式
all： 默认值
```

### 可过渡的样式

不是所有的CSS样式值都可以过渡，只有具有中间值的属性才具备过渡效果

```
Vstart = 开始值; Vend = 结束值; Vres = 中间值; p = 过渡函数的输出值;
Vres = (1-p) * Vstart + p * Vend;
当Vres具有有效值时，则该CSS样式可过渡
```

```
颜色: color background-color border-color outline-color
位置: backround-position left right top bottom
长度: 
    [1]max-height min-height max-width min-width height width
    [2]border-width margin padding outline-width outline-offset
    [3]font-size line-height text-indent vertical-align  
    [4]border-spacing letter-spacing word-spacing
数字: opacity visibility z-index font-weight zoom
组合: text-shadow transform box-shadow clip
其他: gradient
```

## 过渡持续时间

单位：秒s或毫秒ms

### `transition-duration`

值: <time>[,<time>]*
初始值: 0s
应用于: 所有元素
继承性: 无
**注意** 该属性不能为负值
	     若该属性为0s 则为默认值，若为0 则为无效值。所以必须带单位
	     该值为单值时，即所有过渡属性都对应同样时间；为多值时，过渡属性按顺序对应持续时间。

## 过渡延迟时间

定义元素属性延迟多少时间后开始过渡效果，该属性的单位是秒s或毫秒ms

### `transition-delay`

值： <time>[,<time>]*
初始值： 0s
应用于： 所有元素
继承性： 无
**注意**1.该属性若为负值，无延迟效果，但过渡元素的起始值将从0变为设定值（设定值=延迟时间+持续时间）。若该设定值小于等于0，则无过渡效果；若该设定值大于0，则过渡元素从该设定值开始完成剩余的过渡效果
	    2.若该属性为0s则为默认值，若为0则为无效值。所以必须带单位
	    3.该值为单值时，即所有过渡属性都对应同样时间；该值为多值时，过渡属性按照顺序对应持续时间

## 过渡时间函数

过渡时间函数用于定义元素过渡属性随时间变化的过渡速度变化效果

### `transition-timing-function`

值： <timing-function>[,<timing-function>]*
初始值： ease
应用于： 所有元素
继承性： 无

### 取值

过渡时间函数共三种取值，分别为关键字、steps函数和bezier函数

### steps函数

Steps步进函数将过渡时间划分成大小相等的时间间隔来运行
steps函数为

```
steps(<integer>[,start | end]?)

<integer>:用来制定间隔个数（该值只能是正整数）
第二个参数： 该参数可选，默认是end，表示开始值保持一次；若参数为start，表示开始不保持
```



![](/Users/fackin/Documents/myblog/source/_posts/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3CSS%E8%BF%87%E6%B8%A1transition/006tNc79ly1g38sihe8w9j30u00ujwmf.jpg)

### 贝塞尔曲线

贝塞尔曲线通过p0-p3四个控制点来控制，其中p0表示(0,0)，p3表示(1,1)。而<transition-timing-function>就通过确定p1(x1,y1)和p2(x2,y2)的值来确定的

![](/Users/fackin/Documents/myblog/source/_posts/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3CSS%E8%BF%87%E6%B8%A1transition/006tNc79ly1g38wu9s09xj30py02gjrj.jpg)

```css
transition-timing-function: cubic-bezier(x1,y1,x2,y2);
```

**注意**  x1y1x2y2都是0到1的值（包括0和1）

### 关键字

关键字其实是bezier函数或steps函数的特殊值

```
ease: 开始和结束慢，中间快。相当于cubic-bezier(0.25,0.1,0.25,1)
linear: 匀速。相当于cubic-bezier(0,0,1,1)
ease-in: 开始慢。相当于cubic-bezier(0.42,0,1,1)
ease-out: 结束慢。相当于cubic-bezier(0,0,0.58,1)
ease-in-out: 和ease类似，但比ease幅度大。相当于cubic-bezier(0.42,0,0.58,1)
step-start: 直接位于结束处。相当于steps(1,start)
Step-end: 位于开始处经过时间间隔后结束。相当于steps(1,end)
```

## 多值

transition的多个属性值用逗号分隔开表示可以同时为多个值设置过渡属性

```css
transition: <single-transition>[,<single-transition>]*

<single-transition>: <transition-property> || <transition duration> || <transition-timing-function> || <transition-delay>

// property为all,timing-function为linear,delay为0s,duration为0s.表示无过渡行为
transition: 0s;

```

1）若不同的transition-property值，对应的transition-delay | transition-timing-function | transition-duration的属性值都相同时，则对应的这些属性设置一个即可

```css
#test1{
    transition-property: width,background;
    transition-delay: 200ms;
    transition-timing-function: linear;
    transition-duration: 2s;
}
/*类似于*/
#test2{
    transition: width 2s linear 200ms,background 2s linear 200ms;
}

```

2) 当transition-property 值的个数多于对应的transition-delay | transition-timing-function | transition-duration的属性值（属性值的个数大于1个）时，将按顺序开始取值

```css
#test1{
    transition-property: width,background,opacity;
    transition-duration: 2s,500ms;
    transition-timing-function: linear,ease;
    transition-delay: 200ms,0s;
}
/*类似于*/
#test2{
    transition: width 2s linear 200ms,background 500ms ease 0s,opacity 2s linear 200ms;
}

```

3)当transition-property 值的个数少于对应的transition-delay | transition-timing-function | transition-duration的属性值个数时，多余的属性值将无效

```css
#test1{
    transition-property: width;
    transition-duration: 2s,500ms;
    transition-timing-function: linear,ease;
    transition-delay: 200ms,0s;
}
/*类似于*/
#test2{
    transition: width 2s linear 200ms;
}

```

4) 当transition-property的值中出现一个无效值，它依然按顺序对应transition的其他属性值（其他属性出现无效值，处理情况也类似）

```css
#test1{
    transition-property: width,wuxiao,background;
    transition-duration: 2s,500ms;
    transition-timing-function: linear,ease;
    transition-delay: 200ms,0s;
}
/*类似于*/
#test2{
    transition: width 2s linear 200ms,background 2s linear 200ms;
}

```

5) 当transition-property的值中，有些值重复出现多次，则以最后出现的值为准，前面所有出现的值都被认定为无效值，但依然按顺序对应transition的其他属性值

```css
#test1{
    transition-property: width,width,background;
    transition-duration: 2s,500ms;
    transition-timing-function: linear,ease;
    transition-delay: 200ms,0s;
}
/*类似于*/
#test2{
    transition: width 500ms ease 0s,background 2s linear 200ms;
}

```

## 过渡阶段

1）过渡开始时间=样式改变的时刻+过渡延迟时间；而过渡结束时间=过渡开始时间+过渡持续时间
2）过渡起始值=过渡前的过渡属性值；过渡结束值=过渡完成后的过渡属性值
3）过渡分为两个阶段：前进（forward）和反向（reverse）。若前进阶段进行一段时间后进入反响阶段，则方向阶段的初始值时前进阶段结束时的瞬间值
4）以hover为例，若在元素非hover态时设置transition，相当于设置的反向状态。而前进和反向是一致的。而如果在元素hover态设置transition，则前进状态以hover态设置的为准，而反向状态以非hover态设置的为准

```css
.test{
    width: 100px;
    transition: 3s;
}    
.test:hover{
    width: 500px;
    transition: 500ms;
}

```

5）如果子元素和父元素过渡属性都一致。若触发子元素过渡时，父元素正在过渡，则将父元素过渡的中间态的值作为子元素过渡的初始值

```css
.box:hover{
    font-size: 50px;
}
.test:hover{
    font-size: 30px;
}

```

```html
<div class="box">
    <div class="test">test</div>
</div>

```

6）若过渡起始值或过渡开始值为auto，则浏览器不会自己计算成具体数字值，而是不发生过渡效果。所以要过渡某些属性，首先需要将其重置成具体数字值
**注意** 低版本webkit内核浏览器存在bug，会产生反向的过渡效果

```css
.test{
    width: 100px;
    -webkit-transition: width 2s;
    transition:width 2s;
}    
.test:hover{
    width:auto;
}

```

7）隐式过渡是指一个属性改变引起另一个属性的改变。如border-width是1em，则font-size改变时，border-width也会相应的改变。**firefox和IE浏览器支持隐式过渡。而webkit内核浏览器不支持隐式过渡。**

```css
.test{
    border: 1px solid black;
    -webkit-transition: font-size 2s;
    transition:font-size 2s;
    font: 20px/100px "宋体";
}    
.test:hover{
    font-size: 40px;
    border-right-width: 1em;
}

```

## 触发方式

一般而言，过渡transition的触发有三种方式，分别是伪类触发、媒体查询触发和javascript触发。其中常用的伪类触发包括:hover、:focus、:active等
1）hover
鼠标悬停触发
2）active
用户单击元素并按住鼠标时触发
3）focus
获得焦点时触发
4）@media触发
符合媒体查询条件时触发

```css
/* 把浏览器的宽度拖动到小于1000px时触发 */
@media (max-width: 1000px){
    .test{
        width: 500px;
    }
}

```

5）点击事件
用户点击元素时触发

```javascript
test.onclick = function(){
    test.style.width = '300px';
    setTimeout(function(){
        test.style.width = '100px';
    },3000);
}

```

## API

关于过渡transition多事件只有一个，是transitionend事件，它发生在过渡事件完成后
**注意**safari2.1-6、ISO3.2-6.1、android2.1-4.3需要使用webkitTransitionEnd事件

### 属性

transitionEnd的事件对象具有以下3个私有属性
propertyName: 发生transition效果的CSS属性名
elapsedTime: 代表发生实际效果的持续时间。若完整进行，则返回完整时间；若中途中断，则返回实际时间
**注意**该属性具有兼容性问题，chrome返回持续时间加延迟时间，而其他浏览器只返回持续时间

```html
<style>
#test{height: 100px;width: 100px;background-color: pink;
    transition: width 1.5s 0.5s;}
#test:hover{width: 200px;}
</style>

<div id="test"></div>
<script>
test.addEventListener("transitionend", myFunction);
function myFunction(e){
    e = e || event;
    test.innerHTML = 'propertyName:' + e.propertyName + '; elapsedTime:' + e.elapsedTime + '; pseudoElement:' + e.pseudoElement;
}
</script>


```

pseudoElement: 如果transition效果发生在伪元素，会返回该伪元素的名称，以“::”开头。如果不发生在伪元素上，则返回一个空字符串
**注意** 若transition效果发生在伪元素上，IE浏览器将不会触发transitionEnd事件

```html
<style>
#test{height: 100px;width: 100px;position: relative;background-color: lightblue;}
#test:before{content:'我是伪元素';position: absolute;height: 100px;width: 100px;background-color: pink;
    left:200px;}
#test:hover:before{width: 200px;transition: width 1.5s 0.5s;}
</style>

<div id="test"></div>
<script>
test.addEventListener("transitionend", myFunction);
function myFunction(e){
    console.log(event)
    e = e || event;
    test.innerHTML = 'propertyName:' + e.propertyName + '; elapsedTime:' + e.elapsedTime + '; pseudoElement:' + e.pseudoElement;
}
</script>

```

### 注意事项

1）过渡分为两个阶段：前进阶段和反向阶段。transitionend事件在前进阶段结束时会触发，在反向阶段结束时也会触发

```javascript
var index = 0;
//兼容低版本safari、IOS、android
test.addEventListener("webkitTransitionEnd", myFunction);
// 标准语法
test.addEventListener("transitionend", myFunction);
function myFunction() {
    index++;
    this.innerHTML = index;
}

```

2）过渡事件触发的次数与transition-property过渡属性的个数有关。过渡属性有几个就会触发几次

```css
transition: width 1s,background-color 1s;
-webkit-transition: width 1s,background-color 1s;

```

3）如果过渡属性是复合属性，如border-width相当于是border-top-width、border-bottom-width、border-left-eidth和border-right-width这四个属性的集合，则过渡事件触发4次
**注意**在低版本webkit内核浏览器里只触发1次

```css
transition: border-width 1s;
-webkit-transition: border-width 1s;

```

4）如果过渡属性是默认值all，则过渡事件的次数是计算后的非复合的过渡属性的个数。如果发生过渡的属性是border-width和width，则经过计算后过渡事件应该触发5次
**注意**在低版本webkit内核浏览器中处理情况也一致

```css
.test{
    width: 50px;
    border: 1px solid black;
    transition:all 1s;
    -webkit-transition: all 1s;
}    
.test:hover{
    border-width: 10px;
    width: 100px;
}

```

5）如果过渡延迟时间为负值，且绝对值大于等于过渡持续时间时，低版本webkit内核浏览器不会产生过渡效果，但会触发过渡事件；而其他浏览器即不会产生过渡效果，也不会触发过渡事件

```css
    transition:width 1s -1s;
    -webkit-transition: width 1s -1s;

```

6）如果过渡属性存在复合属性及该复合属性包含的非复合属性，则浏览器计算复合属性的子属性时，不会重复计算已包含的属性
**注意** 低版本webkit内核浏览器会出现bug，不仅复合属性被当作一个属性来触发事件，而且会多触发一次

```css
.test{
    border: 1px solid black;
    transition:border-width 1s,border-left-width 2s;
    -webkit-transition: border-width 1s,border-left-width 2s;
}    
.test:hover{
    border-width:10px;
}

```

**注意** 当过渡事件执行完后，应及时使用removeEventListener取消绑定，以免对其他效果造成影响

```javascript
var index = 0;
//兼容低版本safari、IOS、android
test.addEventListener("webkitTransitionEnd", myFunction);
// 标准语法
test.addEventListener("transitionend", myFunction);
function myFunction() {
    index++;
    this.innerHTML = index;
    if(index == 1){
        test.removeEventListener("webkitTransitionEnd", myFunction);
        test.removeEventListener("transitionend", myFunction);
    }
}

```

（完）