# 圣杯布局与双飞翼布局

## 圣杯布局

```html
<body id="body">
    <div id="header">#header</div>
    <div id="container">
      <div id="center" class="column"></div>
      <div id="left" class="column">#left</div>
      <div id="right" class="column">#right</div>
    </div>
    <div id="footer">#footer</div>
  </body>
```

```css
#header,
#footer {
  background: rgba(29, 27, 27, 0.726);
  height: 60px;
}
#container {
  padding-left: 200px; /* leftContent width */
  padding-right: 150px; /* rightContent width */
  overflow: hidden;
}
#container .column {
  position: relative;
  float: left;
  padding-bottom: 10000px;
  margin-bottom: -10000px;
}
#center {
  width: 100%;
  background: rgb(206, 201, 201);
  // padding: 20px; // 如果要使用padding需要将盒模型设置成ie盒模型
}
#left {
  width: 200px; /* leftContent width */
  right: 200px; /* leftContent width */
  margin-left: -100%;
  background: rgba(95, 179, 235, 0.972);
}
#right {
  width: 150px; /* rightContent width */
  margin-right: -150px; /* rightContent width */
  background: rgb(231, 105, 2);
}
```

### 双飞翼布局

container的padding取消,在center的子元素上加上padding.

还有另一种解决方法就是用border-box;



在这个模型中需要注意的一些问题:

### position

设置position后,层级会提升1级

- 定位元素参照**包含块**来定位,没有参照的就按照**初始包含块**

- left top right bottom width height 默认值是auto

- margin padding 默认值是0

- border-width 默认中等大小

- 百分比参照于父级包含块的**宽度**

> 根元素(<html>)所在的包含块是一个被称为**初始包含块**的矩形。即整个网页的viewport
>
> 参考 : [布局和包含块](https://developer.mozilla.org/zh-CN/docs/Web/CSS/All_About_The_Containing_Block)

### float

浮动提升0.5级

- 当开启浮动,一个元素会分两层,上层是文字相关,下层是盒模型相关
- 如果设置浮动,盒模型层会浮动,文字层在原位置

### 等高布局

利用padding-bottom:10000px;margin-bottom:-10000px;父级元素overflosw:hidden来构建等高布局

## 居中问题

### 行内元素水平居中

text-align:center;

line-hight:divHight;

### 已知大小div水平居中

postion:absolute;

Left:50%;

Right:50%;

Mrgin-left:-OwnSize;

Mrgin-top:-OwnSize;

### 未知高度水平居中

postion:absolute;

Left:50%;

Right:50%;

Transform:translate3d(-50%,-50%,0)

### img居中

```less
.right {
  background-color: goldenrod;
  height: 300px;
  overflow: hidden;
  text-align: center;
  img {
    vertical-align: middle;
  }
  &:after {
    content: "";
    display: inline-block;
    height: 100%;
    width: 0;
    vertical-align: middle;
  }
}
```



## Grid布局

## BFC

## 选择器

## Flex

## 1px物理像素实现

像素比 = 物理像素/CSS像素

在高清屏中 DPR 可能会是 2 或者 3 ，那么原先 1px 像素的线在高清屏下就占了2个或者3个物理像素，导致线看着比较粗。

解决：

1. 使用伪类，设置 border 1px scale(0.5)
2. 设置 meta initial-scale 根据DPR设置初始值
3. 伪类 + transform: scaleY(0.5);

```css
/* 伪类 + transform: scaleY(0.5); */
.px1 {
    position: relative;
}

.px1:before {
    content: " ";
    position: absolute;
    left: 0;
    top: 0;
    width: 200%;
    border: 1px solid black;
    color: black;
    height: 200%;
    transform-origin: left top;
    transform: scale(0.5);
}
```

## px rem em vh vw之间的区别

**一、px**

px就是pixel像素的缩写，相对长度单位，网页设计常用的基本单位。像素px是相对于显示器屏幕分辨率而言的。

**二、em**

em是相对长度单位。相对于当前对象内文本的字体尺寸（参考物是父元素的font-size）

如当前父元素的字体尺寸未设置，则相对于浏览器的默认字体尺寸

**特点：**

  1. em的值并不是固定的

  2. em会继承父级元素的字体大小

**三、rem**

rem是CSS3新增的一个相对单位，rem是相对于HTML根元素的字体大小（font-size）来计算的长度单位

**优点**：只需要设置根目录的大小就可以把整个页面的成比例的调好

**rem兼容性**：除了IE8及更早版本外，所有浏览器均已支持rem

如果你没有设置html的字体大小，就会以浏览器默认字体大小，一般是16px

**em与rem的区别：**

rem是相对于根元素（html）的字体大小，而em是相对于其父元素的字体大小

**两者使用规则：**

如果这个属性根据它的font-size进行测量，则使用em 其他的一切事物属性均使用rem

这里有一个px、em、rem单位的转换工具：[pxtorem.com/](http://pxtorem.com/)

**四、vw、vh**

vw、vh、vmax、vmin这四个单位都是基于视口

vw是相对视口（viewport）的宽度而定的，长度等于视口宽度的1/100

假如浏览器的宽度为200px，那么1vw就等于2px（200px/100）

vh是相对视口（viewport）的高度而定的，长度等于视口高度的1/100

假如浏览器的高度为500px，那么1vh就等于5px（500px/100）

vmin和vmax是相对于视口的高度和宽度两者之间的最小值或最大值




## 响应式与自适应

自适应（Adaptive）
   程序代码**主动地**去根据不同的屏幕大小，去实现不同的样式代码，**需要实现不同的样式代码**。

响应式（Responsive）
   程序代码**被动地**去适应屏幕的宽度变化，经常使用**百分比或者media查询**。
 网上流传最多的图就是下面的图，个人认为还是可以能够解释两者之间的区别

![img](https://pic.4sus2.com/uPic/1601699258721B8kfSb.png)



---

