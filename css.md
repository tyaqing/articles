# 样式基础

默认是什么,是否可继承

样式表->规则->选择器+声明块->css属性+css属性组成的键值对

```css
style.css /*样式表*/ 
div ul li .item{}  /* 规则 */ 
/* div ul li .item  选择器*/
div ul {
  /*声明块*/
  /* css属性 键值对 */
  font-size:12px;
}
```

## 选择器

### 基本选择器

- [元素选择器](https://developer.mozilla.org/zh-CN/docs/CSS/Type_selectors) `elementname(元素名称)`
- [类选择器](https://developer.mozilla.org/zh-CN/docs/CSS/Class_selectors) `.classname(类名)`
- [ID 选择器](https://developer.mozilla.org/zh-CN/docs/CSS/ID_selectors) `#idname(ID 名)`
- [通配选择器](https://developer.mozilla.org/zh-CN/docs/CSS/Universal_selectors) `*`, `ns|*`, `*|*`, `|*`
- [属性选择器](https://developer.mozilla.org/zh-CN/docs/CSS/Attribute_selectors) `[属性=值]`

> 笔者不推荐使用通配选择器,因为它是[性能最低的一个CSS选择器](http://www.stevesouders.com/blog/2009/06/18/simplifying-css-selectors/).

### 组合选择器

- [相邻兄弟选择器](https://developer.mozilla.org/zh-CN/docs/CSS/Adjacent_sibling_selectors) `A + B`
- [普通兄弟选择器](https://developer.mozilla.org/zh-CN/docs/CSS/General_sibling_selectors) `A ~ B`
- [子选择器](https://developer.mozilla.org/zh-CN/docs/CSS/Child_selectors) `A > B`
- [后代选择器](https://developer.mozilla.org/zh-CN/docs/CSS/Descendant_selectors) `A B`

### 伪类

**链接伪类**:

- :link  // 仅链接可用

- :visited // 访问后的属性

> 这个样式可能会被后声明的其他链接相关的伪类覆盖，这些伪类包括 ([`:link`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:link), [`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover),和[`:active`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:active))。要适当地设置链接样式，请将`:visited` 规则放在`:link` 规则之后，但在`:hover` 和`:active` 规则之前，参照 LVHA *顺序*:`:link` — `:visited` — `:hover` — `:active`。
>
> 出于隐私原因，浏览器严格限制您可以让此伪类应用的样式，以及使用它们的方式：
>
> - 允许使用的 CSS 属性为[`color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color), [`background-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-color), [`border-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-color), [`border-bottom-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-bottom-color), [`border-left-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-left-color), [`border-right-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-right-color), [`border-top-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-top-color), [`column-rule-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-rule-color), 和[`outline-color`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-color)。
> - [其他参考MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:visited)

**动态伪类:**

- :hover // 鼠标滑过属性

- :active // 鼠标点击下去的属性

**表单伪类**

- :enable // 可编辑

- :disabled //不可编辑

- :checked // 被选中

- :focus //鼠标焦点

**结构性伪类**

- :nth-child(index)  

  - 这个 [CSS 伪类](https://developer.mozilla.org/en-US/docs/CSS/Pseudo-classes)首先找到**所有当前元素**的兄弟元素，然后按照位置先后顺序从1开始排序，选择的结果为CSS伪类:nth-child括号中表达式（an+b）匹配到的元素集合（n=0，1，2，3...）

  - 比如p:nth-child(2)也就是说,它不在乎你第2个元素是什么,我就要找到第2个元素,看是不是p元素,如果不是那就拉倒,如果是,加上样式.

  - 部分常用的示例

    ```
    tr:nth-child(2n+1)
    ```

    表示HTML表格中的奇数行。

    ```
    tr:nth-child(odd)
    ```

    表示HTML表格中的奇数行。

    ```
    tr:nth-child(2n)
    ```

    表示HTML表格中的偶数行。

    ```
    tr:nth-child(even)
    ```

    表示HTML表格中的偶数行。

    ```
    span:nth-child(0n+1)
    ```

    表示子元素中第一个且为span的元素，与 [`:first-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-child) 选择器作用相同。

    ```
    span:nth-child(1)
    ```

    表示父元素中子元素为第一的并且名字为span的标签被选中

    ```
    span:nth-child(-n+3)
    ```

    匹配前三个子元素中的span元素。

  - first-child

  - last-child

  - nth-last-child //倒着数

  - only-child  //匹配没有任何兄弟元素的元素.等效的选择器还可以写成 `:first-child:last-child`或者`:nth-child(1):nth-last-child(1)`,当然,前者的权重会低一点.

- :nth-of-type(index)

  - 这个 [CSS 伪类](https://developer.mozilla.org/en-US/docs/CSS/Pseudo-classes)是针对具有一组兄弟节点的标签, 用 n 来筛选出在一组兄弟节点的位置。
  - 比如p:nth-of-type(3)也就是说,他首先找到子类的所有p元素,然后找到p的第3个元素加样式
  - first-of-type
  - last-of-type
  - nth-last-of-type
  - only-of-type // 即使子元素有很多个,但是只要满足的子元素只有一个就会被选中

- :not

  - 用来匹配不符合一组选择器的元素。由于它的作用是防止特定的元素被选中，它也被称为*反选伪类*（*negation pseudo-class*）  
  - 比如 :not(:last-child) 就是除了最后一个元素,其他元素都加上样式

- :empty // 匹配内容是空的

> 注意
>
> - index是从1开始数的
> - nth-of-type 和 nth-child的选择需要区分

### 伪元素

- ::after

- ::before

  - ```
    /* Add an arrow after links */
    a::after {
      content: "→";
    }
    ```

- :root //根元素

### 基本选择器扩展

```css
#wrap > li  /* 子类选择器,只找子类 */
#wrap > #first + .inner  /*相邻兄弟选择器 只会找#first相邻的.inner*/
#wrap #first ~ .inner /*通用兄弟选择器,匹配所有的.inner*/
```



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

