# JavaScript

## 预解释与变量提升

### JavaScript预解释

我们可以大致把JavaScript在浏览器中运行的过程分为两个阶段`预解释阶段`（有人说准确的说法是应该是Parser，我们以预解释方便理解） `执行阶段`,在JavaScript引擎对JavaScript代码进行执行之前,需要进行预先处理,然后再对处理后的代码进行执行。

> 我们平时书写的JavaScript代码并不是JavaScript执行的代码(V8引擎读取一行执行一行这种理解是错误的),它需要预解释后,再由引擎进行执行.

具体的解释过程涉及到浏览器内核的技术不属于前端领域,不过我们可以浅显的理解一下V8在处理JavaScript的一般过程:

以上例中的`var a = 2;`为例,我们一般人的理解为**声明了一个值为2的变量a**,但是在JavaScript引擎处理时却分为了两个步骤:

> 1. 读取`var a`后,在当前作用域中查找是否有相同声明,如果没有就在当前作用域集合中创建一个名为`a`的变量,否则忽略此声明继续进行解析.
> 2. 接下来,V8引擎会处理`a = 2`的赋值操作,首先会询问当前作用域中是否有名为`a`的变量,如果有进行赋值,否则继续向上级作用域询问.

### JavaScript执行环境

我们上面提到的所谓javascript预解释正是创建函数的**执行环境**（又称“执行上下文”），只有搞定了javascript的执行环境我们才能搞清楚一段代码在执行过后为什么产生这样的结果。

我们用一段伪代码表示创立的**执行环境**

```javascript
executionContextObj = {
    'scopeChain': { /* 变量对象 + 所有父级执行上下文中的变量对象 */ },
    'variableObject': { /*  函数参数 / 参数, 内部变量以及函数声明 */ },
    'this': {}
}
```

作用域链(scopeChain)包括下面提到的变量对象(variableObject)和所有父级执行上下文中的变量对象.

变量对象(variableObject)是与执行上下文相关的数据作用域,一个与上下文相关的特殊对象，其中存储了在上下文中定义的变量和函数声明:

- 变量
- 函数声明
- 函数的形参

在有了这些基板概念之后我们可以梳理一下js引擎创建执行的过程:

- 创建阶段
  - 创建Scope chain
  - 创建variableObject
  - 设置this
- 执行阶段
  - 变量的值、函数的引用
  - 执行代码

而变量对象的创建细节如下:

- 根据函数的参数，创建并初始化arguments object
- 扫描函数内部代码，查找函数声明（Function declaration）
  - 对于所有找到的函数声明，将函数名和函数引用存入变量对象中
  - 如果变量对象中已经有同名的函数，那么就进行覆盖
- 扫描函数内部代码，查找变量声明（Variable declaration）
  - 对于所有找到的变量声明，将变量名存入变量对象中，并初始化为"undefined"
  - 如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性

### 变量提升

```js
var a= 1;
function f() {
  console.log(a);
  var a = 2;
}
f();
```

正是由于以上的处理,产生了大家熟知的JavaScript中的**变量提升**,具体以上代码的执行过程如以下伪代码所示:

```js
// global context
executionContextObj = {
    'scopeChain': { ... },
    'variableObject': { a: undefined, f: pointer to function f() },
    'this': {...}
}
...
}//首先在全局执行环境中声明了变量a以及函数f,此时a虽然被声明,但是尚未赋值
x = 1;
function f() {
    executionContextObj {
    'scopeChain': { ... },
    'variableObject': {
    arguments: {},
    a: undefined
        },
    'this': {...}
    }
    //内部词法环境中声明了变量a,此时a虽然被声明,但是尚未赋值
    console.log(a);//此时a需要被被打印出来,在作用域内寻找a变量赋值,于是被赋值undefined
    a = 2;
}
```

我们可以明显看到,`a`变量在预解释阶段已经被赋值`undefined`,在执行阶段js是自上而下单线执行，当`console.log(a)`执行之时,`a=2`还没有被执行,`a`变量的值便是预处理阶段被赋予的`undefined`,

### 函数声明与函数表达式

我们看到,在编译器处理阶段,除了被`var`声明的变量会有变量提升这一特性之外,函数也会产生这一特性,但是函数声明与函数表达式两种范式创建的函数却表现出不同的结果.

我们先看一个实例,运行以下代码

```javascript
f();
g();
//函数声明
function f() {
    console.log('f');
}
//函数表达式
var g = function() {
    console.log('g');
};
```

`f`成功被打印出来,而`g函数`出现了类型错误,这是什么原因呢?

```javascript
executionContextObj = {
    'scopeChain': { ... },
    'variableObject': { f: pointer to function f(), g: undefined},
    'this': {...}
}

f();
g();
//函数声明
function f() {
    console.log('f');
}
//函数表达式
var g = function() {
    console.log('g');
};
```

我们看到,在预解释阶段函数声明的`f`是被指向了正确的函数得以执行,而函数表达式`g`被赋予`undefined`,`undefined`无法被当作函数执行因此报错`g is not a function`.

### 变量提升与函数提升的优先级

- 函数提升优先级高于变量提升，且不会被同名变量声明覆盖，但是会被变量赋值后覆盖。

- 而且存在同名函数与同名变量时，优先执行函数。

```js
console.log(a);      //f a()
console.log(a());      //1  
var a=1;
function a(){
    console.log(1);
}
console.log(a);       //1   
a=3
console.log(a())      //a not a function
```

它的过程就相当于

```js
var a=function (){   //声明一个变量a指向
    console.log(1)
}
var a;
console.log(a)  //如果a不优先执行含函数，这里是返回undefined;
console.log(a())
a=1    //这里函数与变量
console.log(a)
a=3
console.log(a())
```




### 一道综合题

```js
function Foo() {
    getName = function () { alert (1); };
    return this;
}
Foo.getName = function () { alert (2);};
Foo.prototype.getName = function () { alert (3);};
var getName = function () { alert (4);};
function getName() { alert (5);}
 
//请写出以下输出结果：
Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```

[题解](https://github.com/Wscats/articles/issues/85)

