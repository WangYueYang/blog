## CSS 面试题

### 1. 介绍一下标准的 CSS 的盒子模型？低版本 IE 的盒子模型有什么不同的？

1. 有两种盒子模型：IE 盒模型（border-box）、W3C 标准盒模型（content-box）
1. 盒模型：分为内容（content）、填充（padding）、边界（margin）、边框（border）四个部分

IE 盒模型和 W3C 标准盒模型的区别：

标准盒模型和 IE 盒模型的区别在于设置 width 和 height 时，所对应的范围不同。标准盒模型的 width 和 height 属性的范围只包含了 content，而 IE 盒模型的 width 和 height 属性的范围包含了 border、padding 和 content。

我们经常用的 `box-sizing:border-box` 就是把标准盒模型转为 IE 盒模型。 [CSS 盒模型详解](https://juejin.cn/post/6844903505983963143)

### 2. css 选择器权重优先级

css 权重优先级的作用：

在同一个元素使用不同的方式，声明了相同的一条或多条 css 规则，浏览器会通过权重来判断哪一种方式的声明，与这个元素最为相关，从而在该元素上应用这个声明方式声明的所有 css 规则。

权重的五个等级（由上到下，递减）

1. !important
1. 行内样式 权重 1000
1. ID 选择器, 权重 100
1. class，属性选择器和伪类选择器，权重 10
   > 属性选择器指的是:根据元素的属性及属性值来选择元素，比如 button 的 type 属性等。伪类选择器: :active :focus 等选项.
1. 标签选择器和伪元素选择器，权重 1
   > 伪元素选择器： :before :after

等级关系：

> !important>行内样式>ID 选择器 > 类选择器 | 属性选择器 | 伪类选择器 > 元素选择器

如何计算优先级：

选择器的特殊性值表述为 4 个部分，用 0,0,0,0 表示。

1. ID 选择器的特殊性值，加 0,1,0,0。
1. 类选择器、属性选择器或伪类，加 0,0,1,0。
1. 元素和伪元素，加 0,0,0,1。
1. 通配选择器\*对特殊性没有贡献，即 0,0,0,0。
1. 最后比较特殊的一个标志!important（权重），它没有特殊性值，但它的优先级是最高的，为了方便记忆，可以认为它的特殊性值为 1,0,0,0,0。

[css 优先级计算规则](https://www.cnblogs.com/wangmeijian/p/4207433.html)

### 3. : 和 :: 伪类和伪元素的区别

: --- 是伪类

可以简单理解为当一个元素达到某种特定的状态时，它能够得到一个类似于 class 的某种样式。当状态改变时，又会失去这个样式。它的功能和 class 有些类似，但他是基于文档之外的抽象 class 所以叫伪类。 比如 :hover、:focus

:: --- 是伪元素

伪元素用于创建一些不在文档树中的元素，并为其添加样式。它们允许我们为元素的某些部分设置样式。比如说，我们可以通过::before 来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。但是为了一些兼容性的写法伪元素也会写成单冒号 :

[总结伪类与伪元素](http://www.alloyteam.com/2016/05/summary-of-pseudo-classes-and-pseudo-elements/)

### 4. 如何让 div 水平垂直居中

1. 使用定位 一

```css
div {
  position: absolute;
  width: 300px;
  height: 300px;
  margin: auto;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: pink; /*方便看效果*/
}
```

2. 使用定位 二

```css
/*确定容器的宽高宽500高300的层设置层的外边距 */
div {
  position: absolute; /*绝对定位*/
  width: 500px;
  height: 300px;
  top: 50%;
  left: 50%;
  margin: -150px 0 0 -250px; /*外边距为自身宽高的一半*/
  background-color: pink; /*方便看效果*/
}
```

3. 使用定位 三

```css
div {
  position: absolute; /*相对定位或绝对定位均可*/
  width: 500px;
  height: 300px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: pink; /*方便看效果*/
}
```

4. 使用 flex 布局

```css
.container {
  display: flex;
  align-items: center; /*垂直居中*/
  justify-content: center; /*水平居中*/
}
.containerdiv {
  width: 100px;
  height: 100px;
  background-color: pink; /*方便看效果*/
}
```

5. 利用 text-align:center 和 vertical-align:middle

```css
.container {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
  text-align: center;
  font-size: 0;
  white-space: nowrap;
  overflow: auto;
  width: 100%;
  height: 100%;
}

.container::after {
  content: '';
  display: inline-block;
  height: 100%;
  vertical-align: middle;
}

.box {
  display: inline-block;
  width: 500px;
  height: 400px;
  background-color: pink;
  white-space: normal;
  vertical-align: middle;
}
```

### 5. flex 布局

基本概念：

采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做 main start，结束位置叫做 main end；交叉轴的开始位置叫做 cross start，结束位置叫做 cross end。

项目默认沿主轴排列。单个项目占据的主轴空间叫做 main size，占据的交叉轴空间叫做 cross size。

#### 容器属性：

##### 1. 决定主轴（main axis）的方向，也就是项目的排列方向：<b> flex-direction </b>

> direction：方向，方位；趋势，动向；的意思。所以 flex-direction 决定项目的排列方向

```css
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}

row（默认值）： 主轴为水平方向，起点在左端
row-reverse： 主轴为水平方向，起点在右端，可以理解为 row 属性的 reverse
column: 主轴为垂直方向，起点在上端。 Column 这个单词也有 纵队，纵行（列）的意思。
column-reverse: 主轴为垂直方向，起点在下端。
```

##### 2. 默认情况下，项目都排在一条轴线上。如果一条轴线排不下，决定如何换行: <b> flex-wrap </b>

> wrap: (使文字）换行

```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}

nowrap（默认）：不换行
wrap：正常换行，也就是第一行在上面
wrap-reverse： 和 wrap 相反，也就是 第一行在下面
```

##### 3. flex-flow

> flow: 流动，流淌；所以 flow 包括排序的方向（流动方向），元素流动时候怎么换行（wrap）

flex-flow 属性是 flex-direction 属性和 flex-wrap 属性的简写形式，默认值为 row nowrap。

```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

##### 4. 定义项目在主轴上的对齐方式: <b> justify-content </b>

> justify: 使（文本）对齐； 所以这个属性的意思是对齐内容。。 吗的还要学英语

```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}

具体对齐方式与轴的方向有关。下面假设主轴为从左到右。

flex-start（默认值）：主轴左对齐 （main start）
flex-end： 主轴右对齐 （main end）
center：居中对齐
space-between：两端对齐，项目之间的间隔都相等
space-around： 没个项目两侧的间隔相等，所以项目之间的间隔比项目与边框的间隔大一倍
```

##### 5. 定义项目在交叉轴上如何对齐: <b> align-items </b>

> align: （使）排成一条直线，使平行。

```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}

flex-start：交叉轴的起点对齐 （cross axis start）
flex-end：交叉轴终点对齐 （cross axis end）
center： 居中对齐
baseline：baseline: 项目的第一行文字的基线对齐。
stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
```

##### 6. 多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用: <b> align-content </b>

> 和 align-items 对应，只不过这个是 content

```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}

flex-start： 与交叉轴的起点对齐
flex-end：与交叉轴的终点对齐
center：与交叉轴的中点对齐
space-between： 和 justify-content 的一样
space-around： 和 justify-content 的一样
chretch（默认值）：
```

对容器属性的总结：

1. 主轴的排列方向
2. 主轴的换行方式
3. 1 和 2 的结合
4. 如何排列内容
5. 5 和 6 交叉轴的对齐方式（一个是一行一个是多行）

#### 项目的属性：

##### 1. 定义项目的排列顺序。数值越小，排列越靠前，默认为 0: <b> order </b>

> order: 顺序，次序；

```css
.item {
  order: <integer>;
}
```

##### 2. 定义项目的放大比例，默认为 0，即如果存在剩余空间，也不放大 <b> flex-grow </b>

> grow: 长大，成长；

```css
.item {
  flex-grow: <number>; /* default 0 */
}
```

如果你不想让 flex 里某个元素按比例放大的话就给 0，其他的数值都是按比例放大

##### 3. 定义了项目的缩小比例，默认为 1，即如果空间不足，该项目将缩小: <b> flex-shrink </b>

> shrink: 使）缩小，减少；

```css
.item {
  flex-shrink: <number>; /* default 1 */
}
```

flex-shrink 属性定义了项目的缩小比例，默认为 1，即如果空间不足，该项目将缩小。如果你不想让某个元素缩小，就把它设置为 0。其他的值就是按比例缩小

##### 4. 定义在分配多余空间之前，项目占据的主轴空间：<b> flex-basis </b>

> bisis: 根据；主要成分（或部分）

```css
.item {
  flex-basis: <length> | auto; /* default auto */
}
```

##### 5. <b> flex </b> 属性

flex 属性是 flex-grow, flex-shrink 和 flex-basis 的简写，默认值为 0 1 auto。后两个属性可选。

```css
.item {
  flex: none | [ < 'flex-grow' 按比例放大> < 'flex-shrink' 按比例缩小>? || <
    'flex-basis' 占据主轴的空间> ];
}
```

该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

##### 6. <b> align-self </b> 属性

允许单个元素有和其他元素不一样的对齐方式，可以覆盖 align-item 属性。默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

该属性可能取 6 个值，除了 auto，其他都与 align-items 属性完全一致。

对元素属性的总结：

1. 按照值进行排序，值小的排前面
1. 按比例放大，默认值是 0 表示不放大
1. 按比例缩小，默认值是 1 ，给 0 的话表示不缩小
1. 相当于给一个初始化的宽度
1. 前面 2, 3, 4 的总和 `flex: null | [2 3 4]` 后面两个额属性可选填
1. 单独给元素设置 align 对齐方式，继承自父元素，没有的话等于 stretch

[阮一峰-Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

### 6.css 多列等高如何实现

1. 利用 padding-bottom|margin-bottom 正负值相抵，不会影响页面布局的特点。设置父容器设置超出隐藏（overflow:hidden），这样父容器的高度就还是它里面的列没有设定 padding-bottom 时的高度，当它里面的任一列高度增加了，则父容器的高度被撑到里面最高那列的高度，其他比这列矮的列会用它们的 padding-bottom 补偿这部分高度差。

```css
.wrapper {
  overflow: hidden;
}

.wrapper div {
  float: left;
  margin: 0 10px -9999px 0;
  padding-bottom: 9999px;
  width: 200px;
}
```

2. 利用 flex 布局中的 align-items 属性默认为 stretch，如果项目未设置高度或设为 auto，将占满整个容器的高度的特性，来实现多列等高。

```css
.wrapper {
  display: flex; /*因为 align-items 默认值是 stretch */
}
```

[常用的多列等高布局收藏](https://juejin.cn/post/6844903615182667789)

### 7. base64 图片的优点和缺点

base64 编码是一种图片处理格式，通过特定的算法将图片编码成一长串字符串，在页面上显示的时候，可以用该字符串来代替图片的 url 属性。

优点：

1. 可以减少一次 HTTP 请求
1. 可像单独图片一样使用，比如背景图片重复使用等

缺点：

1. 编码后的文件大小会比之前大 1/3，会增加 css 文件的体积，影响文件加载的速度，增加浏览器对 HTML/CSS 文件解析渲染的时间

2. 使用 base64 无法直接缓存，智能缓存包含 base64 的文件

所以通常用在一些很小的图片，并且在整个网站复用性很高的图片。
[玩转图片 base64 编码](https://www.cnblogs.com/coco1s/p/4375774.html)

### 8. 什么是 BFC

BFC 指的是块级格式化上下文，一个元素形成了 BFC 之后，那么它内部元素产生的布局不会影响到外部元素，外部元素的布局也不会影响到 BFC 中的内部元素。一个 BFC 就像是一个隔离区域，和其他区域互不响。

#### 触发 BFC

只要满足一下任意一项就可以出发 BFC 特性

1. body 根元素
2. 浮动元素： float 除了 none 以外的值
3. 绝对定位元素：position
4. display 为 inline-block、table-cells、flex
5. overflow 除了 visible 以外的值 (hidden、auto、scroll)

#### 解决了哪些问题

1. 清除内部浮动
2. 清除外浮动
3. 组织外边距重叠（margin 重叠）

[块级格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context) |
[10 分钟理解 BFC 原理](https://zhuanlan.zhihu.com/p/25321647)

### 9. IFC 是什么？

IFC 指的是行级格式化上下文，它有这样的一些布局规则：

1. 行级上下文内部的盒子会在水平方向，一个接一个地放置。
1. 当一行不够的时候会自动切换到下一行。
1. 行级上下文的高度由内部最高的内联盒子的高度决定。

### 10. 清除浮动

1. 结尾空元素或者 after 等伪元素或者 br 来 clear
1. 父元素同样浮动
1. BFC

### 11. 媒体查询

@media 用于做响应式适配，会根据不痛的窗口尺寸，展示不同的样式[CSS3 媒体查询](https://www.runoob.com/cssref/css3-pr-mediaquery.html)

### 12. CSS 性能优化

加载性能：

1. css 压缩：将写好的 css 进行打包压缩，可以减少很多的体积。
2. css 单一样式：当需要下边距和左边距的时候，很多时候选择:margin:top 0 bottom 0;但 margin-bottom:bottom;margin-left:left;执行的效率更高。
3. 减少使用@import,而建议使用 link，因为后者在页面加载时一起加载，前者是等待页面加载完成之后再进行加载。

选择器性能：

1. 避免使用通配规则，如\*{}计算次数惊人！只对需要用到的元素进行选择。
1. 如果规则拥有 ID 选择器作为其关键选择器，则不要为规则增加标签。过滤掉无关的规则（这样样式系统就不会浪费时间去匹配它们了）
1. 尽量少的去对标签进行选择，而是用 class。
1. 尽量少的去使用后代选择器，降低选择器的权重值。后代选择器的开销是最高的，尽量将选择器的深度降到最低，最高不要超过三层，更多的使用类来关联每一个标签元素。
1. 尽可能使用样式的继承

渲染性能：

1. 尽量减少页面重排、重绘。
1. 属性值为 0 时，不加单位。
1. 标准化各种浏览器前缀：带浏览器前缀的在前。标准属性在后。

### 13. 有一个高度自适应的 div，里面有两个 div，一个高度 100px，希望另一个填满剩下的高度

方案一：

利用 IE 盒模型，父元素设置 padding-top: 100px 第一个字元素通过 margin-top: -100px 把自己挤到 父元素的 padding 的位置，也就是说第一个字元素实际占用的是 padding 的空间

```css
.wrapper {
  height: 100%;
  padding-top: 100px;
  box-sizing: border-box;
}
.child1 {
  height: 100px;
  margin-top: -100px;
  background: red;
}
.child2 {
  height: 100%;
  background: blue;
}
```

方法二：

第一个子元素浮动，第二个子元素设置 padding。

```css
.wrapper {
  height: 100%;
  box-sizing: border-box;
}
.child1 {
  height: 100px;
  width: 100%;
  float: left;
  background: red;
}
.child2 {
  height: 100%;
  box-sizing: border-box;
  padding-top: 100px;
  background: blue;
}
```
方法三：

和方法一差不多，只是用定位让第一个子元素占据父元素 padding 的位置
