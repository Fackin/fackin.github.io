# Flex布局

学习阮一峰老师的博客：[http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

[demos](https://fackin.github.io/example/flexBoxDemos/index)

## Flex 布局 is What？

Flex 是Flexible Box的缩写，"弹性布局"，任何一个容器都可以指定为Flex布局。

```css
.box {
  dislpay: flex;
}
```

行内元素也可以使用

```css
.box {
  display: inline-flex;
}
```

设为 Flex 布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效。

## 基本概念

采用Flex布局的元素，称为Flex容器（flex container)，简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。

## 容器的属性

```
flex-direction
flex-wrap
flex-flow
justify-content
align-items
align-content
```

### 1. flex-direction属性

`flex-direction`属性决定主轴的方向，即项目的排列方向。

```
row（默认值）：主轴为水平方向，起点在左端。
row-reverse：主轴为水平方向，起点在右端。
column：主轴为垂直方向，起点在上沿。
column-reverse：主轴为垂直方向，起点在下沿。
```

### 2. flex-wrap属性

默认情况下，项目都排在一条线（又称"轴线"）上。`flex-wrap`属性定义，如果一条轴线排不下，如何换行。

```
nowrap（默认）：不换行。
wrap：换行，第一行在上方。
wrap-reverse：换行，第一行在下方。
```

### 3. flex-flow属性

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

### 4.  justify-content属性

`justify-content`属性定义了项目在**主轴**上的对齐方式。

```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

下面假设主轴为从左到右。

```
flex-start（默认值）：左对齐
flex-end：右对齐
center： 居中
space-between：两端对齐，项目之间的间隔都相等。
space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
```

### 5. align-items属性

`align-items`属性定义项目在**交叉轴**上如何对齐。

```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}

```

### 6. align-content属性

`align-content`属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}

```

```
flex-start：与交叉轴的起点对齐。
flex-end：与交叉轴的终点对齐。
center：与交叉轴的中点对齐。
space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
stretch（默认值）：轴线占满整个交叉轴。

```



## 项目的属性

```
order
flex-grow
flex-shrink
flex-basis
flex
align-self

```

### 1.order属性

`order`属性定义项目的排列顺序。数值越小，排列越靠前，默认为0.

```css
.item {
  order: <integer>;
}

```

### 2.flex-grow属性

`flex-grow`属性定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大。

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

```css
.item {
  flex-grow: <number>; /* default 0 */
}

```

### 3.flex-shrink属性

`flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目为0，其他项目都为1，则空间不足时，前者不缩小。

负值对该属性无效。

```css
.item {
  flex-shrink: <number>; /* default 1 */
}

```

### 4. flex-basis属性

`flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。

它可以设为跟width或height属性一样的值，则项目将占据固定空间。

```css
.item {
  flex-basis: <length> | auto; /* default auto */
}

```

### 5. flex属性

`flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。

该属性有两个快捷值：auto(1 1 auto)和 none(0 0 auto)。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}

```

### 6. align-self属性

`align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

该属性可能取6个值，除了auto，其他都与align-items属性完全一致。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}

```



（完）