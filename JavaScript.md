# 预解释与变量提升

## JavaScript预解释

我们可以大致把JavaScript在浏览器中运行的过程分为两个阶段`预解释阶段`（有人说准确的说法是应该是Parser，我们以预解释方便理解） `执行阶段`,在JavaScript引擎对JavaScript代码进行执行之前,需要进行预先处理,然后再对处理后的代码进行执行。

> 我们平时书写的JavaScript代码并不是JavaScript执行的代码(V8引擎读取一行执行一行这种理解是错误的),它需要预解释后,再由引擎进行执行.

具体的解释过程涉及到浏览器内核的技术不属于前端领域,不过我们可以浅显的理解一下V8在处理JavaScript的一般过程:

以上例中的`var a = 2;`为例,我们一般人的理解为**声明了一个值为2的变量a**,但是在JavaScript引擎处理时却分为了两个步骤:

> 1. 读取`var a`后,在当前作用域中查找是否有相同声明,如果没有就在当前作用域集合中创建一个名为`a`的变量,否则忽略此声明继续进行解析.
> 2. 接下来,V8引擎会处理`a = 2`的赋值操作,首先会询问当前作用域中是否有名为`a`的变量,如果有进行赋值,否则继续向上级作用域询问.

## JavaScript执行环境

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

## 变量提升

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

## 函数声明与函数表达式

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

## 变量提升与函数提升的优先级

- 函数提升优先级高于变量提升，且不会被同名变量声明覆盖，但是会被变量赋值后覆盖。

- **而且存在同名函数与同名变量时，优先执行函数。**

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




## 一道综合题

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

## 作用域链机制

作用域就是变量和函数的可访问范围，控制这个变量或者函数可访问行和生命周期

![截屏2020-09-23 下午1.57.40](https://pic.4sus2.com/uPic/1602120606916Bv6upK.png)

在函数执行的时候会创建执行环境，也就是**执行上下文**，在上下文中有个大对象，保存执行环境定义的变量和函数，在使用变量的时候，就会访问这个大对象，这个对象会随着函数的调用而创建，函数执行结束出栈而销毁，那么这些大对象组成一个链，就是作用域链。

## let const var 区别

**var:** 解析器在对js解析时，会将脚本扫描一遍，将变量的声明提前到代码块的顶部，赋值还是在原先的位置，若在赋值前调用，就会出现暂时性死区，值为 `undefined`

**let const：**不存在在变量提升，且作用域是存在于**块级作用域**下，所以这两个的出现解决了变量提升的问题，同时引用块级作用域。
注：变量提升的原因是为了解决函数互相调用的问题。

## 闭包

闭包：定义在一个函数内部的函数，内部函数持有外部函数内变量的引用，这个内部的函数有自己的执行作用域，可以避免外部污染。

关于闭包的理解，可以说是一千个读者就有一千个哈姆雷特，找到适合自己理解和讲述的就行。

场景有：

1. 函数式编程，compose curry
2. 函数工厂、单利
3. 私有变量和方法，面向对象编程

# 原型链

原型链一般指的是实例对象的隐式原型链.

![js-object-layout](https://pic.4sus2.com/uPic/1602120002818zUfeK4.png)

- 每个函数都有自己的**prototype**,每个实例对象都有一个**\__proto__**
- **对象的隐式原型的值是对应构造函数显式原型的值**
  - 函数的prototype的值是定义函数时候添加的,默认是一个空object对象
  - 对象的\__proto__是创建时候添加的,默认是构造函数的prototype属性值
  - prototype中有一个属性constructor,他指向函数对象


## 原型链

### 作用

- **实现继承**：如果没有原型链，每个对象就都是孤立的，对象间就没有关联，所以原型链就像一颗树干，从而可以实现面对对象中的继承
- **属性查找**：首先在当前实例对象上查找，要是没找到，那么沿着 `__proto__` 往上查找
- **实例类型判断**：判断这个实例是否属于某类对象

### 总结

- Function是它自己的实例,所以他的prototype和\__proto__是一样的
- 所有构造函数都是Function的实例,所有的对象都是Object的实例.
  - Function instanceof Function
  - Object instanceof Object
  - Function instanceof Object
  - Object instanceof Function
- Object的原型对象是原型链的尽头null

## 题目

```js
		function Per() {}

    Per.prototype = {
      num1: 1000,

      money: {
        num2: 1000,
      },

      buy: function () {
        console.log(this.num1, this.money.num2);
      },
    };

    var p1 = new Per();

    var p2 = new Per();

    p2.num1 = 0;

    p2.money.num2 = 0;

    p1.buy();
```



# 构造函数

只要能通过new来调用的就可以作为构造函数.

## new的过程

1. 创建一个空对象 obj ，分配内存空间
2. 从参数列表中获取构造函数，并将 `obj` 的 `__proto__` 属性指向构造函数的 `prototype`
3. 通过 `apply` 执行构造，并将当前 `this` 的指向改为 `obj`
4. 返回构造函数的执行结果，或者当前的 `obj` 对象

```js
function Person(name, age) {
  this.name = name;
  this.age = age;

  this.getName = () => {
    return this.name;
  };
}
function myNew(Constructor, ...argus) {
  const obj = {}; // 此时obj的prototype已经创建了
  obj.__proto__ = Constructor.prototype; // 将隐式原型链指向Person的显示原型链上
  let res = Constructor.apply(obj, argus);
  // console.log(Constructor, res, argus);
  // 这里怕Constructor返回了一个对象
  return typeof res === "object" ? res : obj;
}
let person1 = myNew(Person, "work1", 32);
let person2 = new Person("person2", 23);
console.log(person1, person2);
```

# 箭头函数

箭头函数是普通函数的简写，可以更优雅的定义一个函数，和普通函数相比，有以下几点差异：

- 函数体内的 this 对象，就是**定义时所在的对象**，而不是使用时所在的对象。

- 不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
  - 在浏览器中箭头函数没有 `arguments`
  - 在 nodejs 中，有 `arguments` ，可通过其获取参数长度，但不能通过改对象获取参数列表

- 不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

- 不可以使用 new 命令，因为：
  - 没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 \__proto__
  - 没有自己的 this，无法调用 call，apply。

# this

- 所有函数内部都有一个变量this,函数内this的属性在constructor上,即函数对象上.
- 他的值是调用函数的当前对象决定的
- 任何函数本质上都是通过某个对象来调用,如果没有指定对象,就是window(严格模式下是undefined)

**绑定类型**:默认绑定,隐式绑定,硬绑定,构造函数绑定

## this在setTimeout中的指向

setTimeout会在宏任务中执行,所以this指向的是window

## 箭头函数中this的指向

箭头函数的this是在定义的时候就决定了,所以不能使用call,apply,bind方法

# 数据类型

**基本数据类型**: String Number BigInt Boolean Symbol  Undefined Null 

**引用类型**: object array function

## typeof 

typeof可以辨别的类型:number boolean string undefined function object

null 显示的object,

## instanceof

用于判断该对象是否是目标实例，根据原型链 `__proto__` 逐层向上查找，通过 instanceof 也可以判断一个实例是否是其父类型或者祖先类型的实例。

## Object.prototype.toString.call 和 ===

所有对象都可以通过Object.prototype.toString.call来分辨类型,但效率不高

undefined 和null 可以用===判断

## 包装对象

包装对象，只要是为了便于基本类型调用对象的方法。

boolean string number symbol

## 基本数据类型和引用类型在内存上的区别

**基本类型**：存储在栈内存中，因为基本类型的大小是固定，在栈内可以快速查找。

**引用类型**：存储在堆内存中，因为引用类型的大小是不固定的，所以存储在堆内存中，然后栈内存中仅存储堆中的内存地址。

## undefined

变量已定义,但未赋值

## 描述null

- null 是基本类型之一，不是 Object 对象
- 表示值未知
- 可以使用===判断

## NaN是什么

- NaN 属性是代表非数字值的特殊值，该属性用于表示某个值不是数字。
- 可直接使用 `Number.isNaN()`判断是不是

## 函数给别人用需要考虑什么

类型判断,取值范围

# class和function的区别

class 也是一个语法糖，本质还是基于原型链，class 语义化和编码上更加符合面向对象的思维。

对于 `function` 可以用 `call apply bind` 的方式来改变他的执行上下文，但是 `class` 却不可以，class 虽然本质上也是一个函数，但在转成 es5 （babel）做了一层代理，来禁止了这种行为。

- class 中定义的方法不可用 `Object.keys()` 遍历
- class 不可以定义私有的属性和方法， function 可以，只要不挂载在 this 作用域下就行
- class 只能通过类名调用
- class 的静态方法，this 指向类而非实例

# 实现继承的方式

class继承

原型链继承

# 数据属性与访问器属性

## 数据属性

- **`writable`** — 如果为 `true`，则值可以被修改，否则它是只可读的。
- **`enumerable`** — 如果为 `true`，则会被在循环中列出，否则不会被列出。
- **`configurable`** — 如果为 `true`，则此特性可以被删除，这些属性也可以被修改，否则不可以。

**Value：**包含这个属性的值。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。这个特性的默认值为 undefined。

> 数据属性可以直接定义，如 `var p = {name:'xxx'}` 这个 `name` 就是数据属性，直接定义下，相关属性值都是 true ，如果要修改默认的定义值，那么使用 `Object.defineProperty()` 方法

- 调用Object.defineProperty()方法时，如果不显示指定configurable，enumerable，writable的值，就默认为false

## 访问器属性

访问器属性不包含数据值，没有 value 属性，有 `get set` 属性，通过这两个属性来对值进行自定义的读和写，可以理解为取值和赋值前的**拦截器**，相关属性如下：

**Configurable：**表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为数据属性，默认 false

**Enumerable：**表示能否通过 for-in 循环返回属性，默认 false

**Get：**在读取属性时调用的函数。默认值为 undefined

**Set：**在写入属性时调用的函数。默认值为 undefined

- 访器属性不能直接定义，必须使用 Object.defineProperty() 来定义。
- 根据 `get set` 的特性，可以实现对象的代理， `vue` 就是通过这个实现数据的劫持。

两者的相同点：都有 Configurable 和 Enumerable 属性。

## 获取属性描述

Object.getOwnPropertyDescriptors(obj)

# 异步编程方案

这个问题出场率很高呀！常见的有如下几个：

- `回调函数`：通过嵌套调用实现
- `Generator`: 异步任务的容器，生成器本质上是一种特殊的迭代器， Generator 执行后返回的是个指针对象，调用对象里的 next 函数，会移动内部指针，分阶段执行 `Generator` 函数 ，指向 `yield` 语句，返回一个对象 {value:当前的执行结果，done:是否结束}
- `promise`: 而是一种新的语法糖， Promise 的最大问题是代码冗余，通过 then 传递执行权，因为需求手动调用 then 方法，当异步函数多的时候，原来的语义变得很不清楚
- `co`: 把 Generator 和 Promise 封装，达到自动执行
- `async\await`: 目前是es7草案，可通过 `bable webpack` 等工具提前使用，目前原生浏览器支持还不太好。其本质上是语法糖，跟 co 库一样，都是对 `generator+promise` 的封装，不过相比 co ，语义化更好，可以像普通函数一样调用，且大概率是未来的趋势。

怎么评判每个异步的的优缺点呢:

1. 阅读性
2. 兼容性

## 回调函数

- 回调函数有一个致命的弱点，就是容易写出**回调地狱（Callback hell）**
- 不利于代码的阅读和维护，各个部分之间高度耦合，使得程序结构混乱、流程难以追踪（尤其是多个回调函数嵌套的情况），而且每个任务只能指定一个回调函数
- 此外它不能使用 try catch 捕获错误，不能直接 return。

## Promise

Promise 已经是 ES6 的规范了，相比 Generator ，设计的更加合理和便捷。

看看Promise的规范：

1. 一个 Promise 的当前状态必须为以下三种状态中的一种：等待态（Pending）、执行态（Fulfilled）和拒绝态（Rejected），状态的改变只能是单向的，且变化后不可在改变。
2. 一个 Promise 必须提供一个 then 方法以访问其当前值、终值和据因。 promise.then(onFulfilled, onRejected) 回调函数只能执行一次，且返回 promise 对象

promise 的每个操作返回的都是 promise 对象，可支持链式调用。通过 then 方法执行回调函数，Promise 的回调函数是放在事件循环中的微队列。

### Promise.all的实现

```js
function promiseAll(promiseArray) {
    return new Promise(function (resolve, reject) {
        if (!Array.isArray(promiseArray)) {
            throw new TypeError(`argument must be a array`)
        }

        let resolvedCounter = 0;
        let promiseNum = promiseArray.length;
        let resolvedResult = [];

        for (let i = 0; i < promiseNum; i++) {
            Promise.resolve(promiseArray[i]).then(
                value => {
                    resolvedCounter++;
                    resolvedResult[i] = value;
                    if (resolvedCounter == promiseNum) {
                        return resolve(resolvedResult)
                    }
                },
                reason => {
                    return reject(error)
                }
            )
        }
    })
}
```

- 比如无法取消 Promise
- 错误需要通过回调函数捕获

## Generator

Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同，Generator 最大的特点就是可以控制函数的执行。

- 语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态。
- **Generator 函数除了状态机，还是一个遍历器对象生成函数**。
- **可暂停函数, yield可暂停，next方法可启动，每次返回的是yield后的表达式结果**。
- yield表达式本身没有返回值，或者说总是返回undefined。**next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值**。

### generator解决了什么问题

原来那种回调异步模式，代码时间长了，回过头来看，有可能出现自己都看不明白的危险。有了generator，同步表达就容易理解得多，但编码的时候还是偏向异步思维方式，需要透彻的理解回调异步。

**最完美的异步模式是没有显式的异步，代码不需要generator/yield或者async/await的包装转换，就是直接写同步代码，思维方式也是同步的，也不需要理解回调异步。**

- 执行流程管理不方便，异步返回的值需要手动传递，编码上较容易出错。

> 关于引入generator的目的 https://cnodejs.org/topic/56ded164255ed94c6e4c26c6

### co库及其源码浅析

[co库](https://github.com/tj/co)是一个利用generator和Promise让异步代码更直观的辅助库，它的使用形式如下。

```js
import co from 'co';

co(function* () {
    const result = yield Promise.resolve(true);
    return result;
}).then(function (value) {
    console.log(value);
}, function (err) {
    console.error(err.stack);
});
```

## Async/Await

所以说`async/await`就是`generator` + `promise`的语法糖

1、async函数自带执行器，自动执行，无需next
2、yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。
3、async函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator,比起星号和yield，语义更清楚了。async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。

- 目前还有部分不兼容,需要babel换成promise才可以用

## 总结

**1.JS 异步编程进化史：callback -> promise -> generator -> async + await**

**2.async/await 函数的实现，就是将 Generator 函数和自动执行器，包装在一个函数里。**

**3.async/await可以说是异步终极解决方案了。**

**(1) async/await函数相对于Promise，优势体现在**：

- 处理 then 的调用链，能够更清晰准确的写出代码
- 并且也能优雅地解决回调地狱问题。

当然async/await函数也存在一些缺点，因为 await 将异步代码改造成了同步代码，如果多个异步代码没有依赖性却使用了 await 会导致性能上的降低，代码没有依赖性的话，完全可以使用 Promise.all 的方式。

**(2) async/await函数对 Generator 函数的改进，体现在以下三点**：

- 内置执行器。 Generator 函数的执行必须靠执行器，所以才有了 co 函数库，而 async 函数自带执行器。也就是说，**async 函数的执行，与普通函数一模一样，只要一行**。
- 更广的适用性。 co 函数库约定，yield 命令后面只能是 Thunk 函数或 Promise 对象，而 **async 函数的 await 命令后面，可以跟 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）**。
- 更好的语义。 async 和 await，比起星号和 yield，语义更清楚了。async 表示函数里有异步操作，await 表示紧跟在后面的表达式需要等待结果。



# 什么是严格模式

通过在脚本的最顶端放上一个特定语句 `"use strict";` 整个脚本就可开启严格模式语法。

严格模式下有以下好处：

1. 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
2. 消除代码运行的一些不安全之处，保证代码运行的安全；
3. 提高编译器效率，增加运行速度；
4. 为未来新版本的Javascript做好铺垫。

如以下具体的场景：

1. 严格模式会使引起静默失败(silently fail,注:不报错也没有任何效果)的赋值操作抛出异常
2. 严格模式下的 eval 不再为上层范围(surrounding scope,注:包围eval代码块的范围)引入新变量
3. 严格模式禁止删除声明变量
4. 在严格模式中一部分字符变成了保留的关键字。这些字符包括implements, interface, let, package, private, protected, public, static和yield。在严格模式下，你不能再用这些名字作为变量名或者形参名。
5. 严格模式下 arguments 和参数值是完全独立的，非严格下修改是会相互影响的

# map 和 weakMap区别

`map` 的 `key` 可以是任意类型，在 `map` 内部有两个数组，分别存放 `key` 和 `value` ，用下标保证两者的一一对应，在对 `map` 操作时，内部会遍历数组，时间复杂度O(n)，其次，因为数组会一直引用每个键和值，回收算法没法回收处理，可能会导致内存泄露。

相比之下， `WeakMap` 的键值必须是对象，持有的是每个键对象的 `弱引用` ，这意味着在没有其他引用存在时垃圾回收能正确进行。

```javascript
const wm1 = new WeakMap();
const o1 = {};
wm1.set(o1, 37);  // 当 o1 对象被回收，那么 WeakMap 中的值也被释放
```

## WeakSet/WeakMap

JavaScript垃圾回收是一种内存管理技术。在这种技术中，不再被引用的对象会被自动删除，而与其相关的资源也会被一同回收。Set中对象的引用都是强类型化的，并不会允许垃圾回收。这样一来，如果Set中引用了不再需要的大型对象，如已经从DOM树中删除的DOM元素，那么其回收代价是昂贵的。

为了解决这个问题，ES6还引入了WeakSet的弱集合。这些集合之所以是“弱的”，是因为它们允许从内存中清除不再需要的被这些集合所引用的对象。

首先让我们了解下WeakSet与Set的不同，以下三点是WeakSet与Set不一样的地方：

1. Set可以存储值类型和对象引用类型，而WeakSet只能存储对象引用类型，否则会抛出TypeError。
2. WeakSet不能包含无引用的对象，否则会被自动清除出集合（垃圾回收机制）。
3. WeakSet对象是不可枚举的，也就是说无法获取大小，也无法获取其中包含的元素。

# 循环有几种，是否支持中断和异步

- for 支持中断、支持异步事件
- for of 支持中断、支持异步事件
- for in 支持中断、支持异步事件
- forEach 不支持中断、不支持异步事件
- map 不支持中断、不支持异步事件，支持异步处理方法：map 返回promise数组，在使用 Promise.all 一起处理异步事件数组
- reduce 不支持中断、不支持异步事件，支持异步处理方法：返回值返回 promise 对象

map 的比较简单就不写了，我写个 `reduce` 处理 `async/await` 的 demo

```javascript
const sleep = time => new Promise(res => setTimeout(res, time))
async function ff(){
    let aa = [1,2,3]
    let pp = await aa.reduce(async (re,val)=>{
        let r = await re;
        await sleep(3000)
        r += val;
        return Promise.resolve(r)
    },Promise.resolve(0))
    console.log(pp) // 6
}
ff()
```

# 进程和线程

首先来一句话概括：进程和线程都是一个时间段的描述，都是对CPU工作时间段的描述。

当一个任务得到 CPU 资源后，需要加载执行这个任务所需要的执行环境，也叫上下文，进程就是包含上下文切换的程序执行时间总和 = CPU加载上下文 + CPU执行 + CPU保存上下文。可见进程的颗粒度太大，每次都需要上下文的调入，保存，调出。

如果我们把进程比喻为一个运行在电脑上的软件，那么一个软件的执行不可能是一条逻辑执行的，必定有多个分支和多个程序段，就好比要实现程序A，实际分成 a，b，c等多个块组合而成。

那么这里具体的执行就是：程序A得到CPU => CPU加载上下文 => 开始执行程序A的a小段 => 然后执行A的b小段 => 然后再执行A的c小段 => 最后CPU保存A的上下文。这里a，b，c 的执行共享了A的上下文，CPU在执行的时候没有进行上下文切换的。

a，b，c 我们就是称为线程，就是说线程是共享了进程的上下文环境，是更为细小的 CPU 执行时间段。

![单线程与多线程的进程对比图](https://pic.4sus2.com/uPic/1602120985176mjaDbU.png)

一个进程就是一个程序的运行实例。详细解释就是，启动一个程序的时候，操作系统会为该程序创建一块内存，用来存放代码、运行中的数据和一个执行任务的主线程，我们把这样的一个运行环境叫**进程**。

线程是依附于进程的，而进程中使用多线程并行处理能提升运算效率。

总结来说，进程和线程之间的关系有以下 4 个特点。

1. 进程中的任意一线程执行出错，都会导致整个进程的崩溃。
2. 线程之间共享进程中的数据。
3. 当一个进程关闭之后，操作系统会回收进程所占用的内存。
4. 进程之间的内容相互隔离。

## 观察者模式、发布-订阅者模式的区别

两者都是订阅-通知的模式，区别在于：

观察者模式：观察者和订阅者是互相知道彼此的，是一个紧耦合的设计

发布-订阅：观察者和订阅者是不知道彼此的，因为他们中间是通过一个订阅中心来交互的，订阅中心存储了多个订阅者，当有新的发布的时候，就会告知订阅者

设计模式的名词实在有点多且绕，我画个简单的图：

![dingyue](https://pic.4sus2.com/uPic/1602121033764IyTMJz.png)

## 深克隆

```js
function deepCopy(obj) {
  // 主要判断基本类型 object function array
  let newObj = Array.isArray(obj) ? [] : {};
  if (obj && typeof obj === "object") {
    for (let key in obj) {
      // 如果子节点是对象继续克隆,如果不是object就直接赋值了
      newObj[key] =
        typeof obj[key] === "object" ? deepCopy(obj[key]) : obj[key];
    }
  }
  return obj;
}
let a = {
  a: {},
  b: [1, 2, 3, { a: { b: "name" } }],
  c: function () {},
};
let d = deepCopy(a);
console.log(d);
```

# 浏览器端数据存储方案

在浏览器端存储数据对我们是很有用，这相当于赋予浏览器记忆的功能，可以纪录用户的所有状态信息，增强用户体验。比如当纪录用户的登陆状态时，可以让用户能够更快的进行访问，而不是每次登陆时都需要去进行繁琐的操作。

总的来说,现在市面上最常见的数据存储方案是以下三种：

- Cookie
- web存储 (localStorage和seesionStorage)
- IndexedDB

![img](https://pic.4sus2.com/uPic/1602121033764CjwulV.png)

## Cookie

Cookie的又称是HTTP Cookie，最初是在客户端用于存储会话信息，从底层来看，它作为HTTP协议的一种扩展实现，Cookie数据会自动在web浏览器和web服务器之间传输，因此在服务器端脚本就可以读写存储的cookie的值，因此Cookie通常用于存储一些通用的数据，比如用户的登陆状态，首选项等。虽然随着时代的进步，HTML5所提供的web存储机制已经逐步替代了Cookie，但有些较为老的浏览器还是不兼容web存储机制，因此正处于这个老旧更替阶段的我们对于它还是要了解了解的。(比如我这个瓜皮还在用它，2333)

### Cookie的优点

首先由于操作Cookie的API很早就已经定义和实现了，因此相比于其他的数据存储方式，Cookie的兼容性非常的好，兼容现在市面上所有的主流浏览器，我们在使用它的时候完全不用担心兼容问题。

### Cookie的缺点

说到Cookie的缺点，那就有点多了，不然也不会在Cookie后面出现web存储等新的数据存储的方案了。 总结起来Cookie的缺点主要是以下几点：

1. 存储量小。虽不同浏览器的存储量不同，但基本上都是在4kb左右。
2. 影响性能。由于Cookie会由浏览器作为请求头发送，因此当Cookie存储信息过多时，会影响特定域的资源获取的效率，增加文档传输的负载。
3. 只能储存字符串。
4. 安全问题。存储在Cookie的任何数据可以被他人访问，因此不能在Cookie中储存重要的信息。
5. 由于第三方Cookie的滥用，所以很多老司机在浏览网页时会禁用Cookie，所以我们不得不测试用户是否支持Cookie，这也是很麻烦的一件事。

### Cookie的操作

基本的Cookie操作主要有三个：读取，写入和删除。但在JavaScript中去处理cookie是一件很繁琐的事情，因为cookie中的所有的名字和值都是经过URI编码的，所以当我们必须使用decodeURICompoent来进行解码才能得到cookie的值。我们来看看CookieUtil对象是如何操纵cookie的：

```
var CookieUtil = {
	// get可根据cookie的名字获取相应的值
	get: function() {
		const cookieName = encodeURIcOMPONET(name) + "=",
			   cookieStart = document.cookie.indexOf(cookieName),
			   cookieValue = null
		if(cookieStart > -1) {
			const cookieEnd = document.cookie.indexOf(";", cookieStart)
			if(cookieEnd == -1) {
				cookieEnd = document.cookie.length
			}
			cookieValue = decodeURICompoent(document.cookie.substring(cookieStart + cookieName.length, cookieEnd))	
		}
		return cookieValue
	}
	// set设置一个cookie
	set: function(name, value, expires, path, domain, secure) {
		var cookieText = encodeURIComponet(name)+"="+encodeURIComponet(value)
		if(expires instanceof Date) {
			cookieText += "; expires=" + expires.toGMTString()
		}
		if(path) {
			cookieText += ";path=" + path
		}
		if(domain) {
			cookieText += "; domain" + domain
		}
		if(secure) {
			cookieText += "; secure"
		}
		document.cookie = cookieText
	}
	// 删除已有的cookie
	unset: function(name, path, domain, secure) {
		this.set(name, "", new Date(0), path, domain, secure)
	}
}
复制代码
```

是不是很麻烦，无论是获取一个cookie的值或是设置一个cookie都是很麻烦的事情，这也成为了后续的浏览器数据存储方案出现的一大原因。

## web存储

web存储机制最初作为HTML5的一部分被定义成API的形式，但又由于其本身的独特性与其他的一些原因而剥离了出来，成为独立的一个标准。web存储标准的API包括locaStorage对象和seesionStorage对象。它所产生的主要原因主要出于以下两个原因：

- 人们希望有一种在cookie之外存储回话数据的途径。
- 人们希望有一种存储大量可以跨会话存在的数据的机制。

（注：其实在最初的web存储规范中包含了两种对象的定义：seesionStorage和globalStorage,这两个对象在支持这两个对象的浏览器中都是以windows对象属性的形式存在的）

### localStorage

localStorage对象在修订过的HTML5规范中作为持久保存客户端数据的方案取代了我们上面所提到的globalStorage。从功能上来讲，我们可以通过locaStorage在浏览器端存储键值对数据，它相比于cookie而言，提供了更为直观的API，且在安全上相对好一点 ，而且虽然localStorage只能存储字符串，但它也可以存储字符串化的JSON数据，因此相比于cookie，localStorage能存储更复杂的数据。总的来说相较于cookie，localStorage有以下优势：

- 提供了简单明了的API来进行操作
- 更加安全
- 可储存的数据量更大

也正是出于以上这些原因，localStorage被视为替代cookie的解决方案，但还是要注意不要在localStorage中存储敏感信息。

### localStorage的基本语法

localStorage的基本操作很简单，示例如下：

```
// 使用方法存储数据
localStorage.setItem("name", "Srtian")
// 使用属性存储数据
localStorage.say = "Hello world"
// 使用方法读取数据
const name = localStorage.getItem("name")
// 使用属性读取数据
const say = localStorage.say
// 删除数据
localStorage.removeItem("name")
复制代码
```

但需要注意的是，我们上面的示例全是存储字符串格式的数据，当我们需要传输其他格式的数据时，我们就需要将这些数据全部转换为字符串格式，然后再进行存储：

```
const user = {name:"Srtian", age: 22}
localStorage.setItem("user", JSON.stringify(user))
复制代码
```

当然，我们在获取值的时候也别忘了将其转化回来：

```
var age = JSON.parse(localStorage.user)
复制代码
```

### localStorage储存数据的有效期与作用域

通过localStorage存储的数据时永久性的，除非我们使用removeItem来删除或者用户通过设置浏览器配置来删除，负责数据会一直保留在用户的电脑上，永不过期。

localStorage的作用域限定在文档源级别的（意思就是同源的才能共享），同源的文档间会共享localStorage的数据，他们可以互相读取对方的数据，甚至有时会覆盖对方的数据。当然，localStorage的作用域同样也受浏览器的限制。

### localStorage的兼容

localStorage的兼容如下表所示：

```
    Feature 	Chrome 	Edge 	Firefox (Gecko) Internet Explorer 	Opera 	Safari (WebKit)
localStorage 	4 	(Yes) 	   3.5 	            8 	             10.50     4
sessionStorage 	5 	(Yes) 	   2 	            8 	             10.50 	   4
复制代码
```

## sessionStorage

sessionStorage是web存储机制的另一大对象，sessionStorage 属性允许我们去访问一个 session Storage 对象。它与 localStorage 相似，不同之处在于 localStorage里面存储的数据没有过期时间设置，而Session Storage只存储当前会话页的数据，且只有当用户关闭当前会话页或浏览器时，数据才会被清除。

#### sessionStorage的基本语法

我们可以通过下面的语法，来保存，获取，删除数据，大体语法与：

```
// 保存数据到sessionStorage
sessionStorage.setItem('name', 'Srtian');

// 从sessionStorage获取数据
var data = sessionStorage.getItem('name');

// 从sessionStorage删除保存的数据
sessionStorage.removeItem('name');

// 从sessionStorage删除所有保存的数据
sessionStorage.clear();
复制代码
```

下面的示例会自动保存一个文本输入框的内容，如果浏览器因偶然因素被刷新了，文本输入框里面的内容会被恢复，写入的内容不会丢失：

```
// 获取文本输入框
var field = document.getElementById("field")

// 检测是否存在 autosave 键值
// (这个会在页面偶然被刷新的情况下存在)
if (sessionStorage.getItem("autosave")) {
  // 恢复文本输入框的内容
  field.value = sessionStorage.getItem("autosave")
}
// 监听文本输入框的 change 事件
field.addEventListener("change", function() {
  // 保存结果到 sessionStorage 对象中
  sessionStorage.setItem("autosave", field.value)
})
复制代码
```

在兼容性和优点方面，sessionStorage和localStorage是差不多的，因此在此也就不多说了，下面我们来聊一聊IndexedDB。

## IndexedDB

虽然web存储机制对于存储较少量的数据非常便捷好用，但对于存储更大量的结构化数据来说，这种方法就不太满足开发者们的需求了。IndexedDB就是为了应对这个需求而产生的，它是由HTML5所提供的一种本地存储，用于在浏览器中储存较大数据结构的 Web API，并提供索引功能以实现高性能查找。它一般用于保存大量用户数据并要求数据之间有搜索需要的场景，当网络断开时，用户就可以做一些离线的操作。它较之SQL更为方便，不需要写一些特定的语法对数据进行操作，数据格式是JSON。

### IndexedDB的基本语法

使用IndexedDB在浏览器端存储数据会比上述的其他方法更为复杂。首先，我们需要创建数据库，并指定这个数据库的版本号：

```
// 注意数据库的版本号只能是整数
const request = IndexedDB.open(databasename, version)
复制代码
```

然后我们需要生成处理函数，需要注意的是onupgradeneeded 是我们唯一可以修改数据库结构的地方。在这里面，我们可以创建和删除对象存储空间以及构建和删除索引。 ：

```
request.onerror = function() {
	// 创建数据库失败时的回调函数
}
request.onsuccess = function() {
	// 创建数据库成功时的回调函数
}
request.onupgradeneededd = function(e) {
	 // 当数据库改变时的回调函数
}

复制代码
```

然后我们就可以建立对象存储空间了，对象存储空间仅调用createObjectStore()就可以创建。这个方法使用存储空间的名称，和一个对象参数。即便这个参数对象是可选的，它还是非常重要的，因为它可以让我们定义重要的可选属性和完善你希望创建的对象存储空间的类型。

```
request.onupgradeneeded = function(event) {
	const db = event.target.result
	const objectStore = db.createObjectStore('name', { keyPath:'id' })
}
复制代码
```

对象的存储空间我们已经建立好了，接下来我们就可以进行一系列的骚操作了，比如来个蛇皮走位！不不不，口误口误，比如添加数据：

```
addData: function(db, storename, data) {
	const store = store = db.transaction(storename, 'readwrite').objectStore(storename)
	for(let i = 0; i < data.length; i++) {
		const request = store.add(data[i])
		request.onerror = function() {
			console.error('添加数据失败')
		}
		request.onsuccess = function() {
			console.log('添加数据成功')
		}
	}
}
复制代码
```

如果我们想要修改数据，语法与添加数据差不多，因为重复添加已存在的数据会更新原本的数据，但还是有细小的差别：

```
putData: function(db, storename, data) {
	const store = store = db.transaction(storename, 'readwrite').objectStore(storename)
	for(let i = 0; i < data.length; i++) {
		const request = store.put(data[i])
		request.onerror = function() {
			console.error('修改数据失败')
		}
		request.onsuccess = function() {
			console.log('修改数据成功')
		}
	}
}
复制代码
```

获取数据：

```
getDataByKey: function(db, storename, key) {
	const store = store = db.transaction(storename, 'readwrite').objectStore(storename)
	const request = store.get(key)
	request.onerror = function() {
		console.error('获取数据失败')
	}
	request.onsuccess = function(e) {
		const result = e.target.result
		console.log(result)
	}
}
复制代码
```

删除数据：

```
deleteDate: function(db, storename, key) {
	const store = store = db.transaction(storename, 'readwrite').objectStore(storename)
	store.delete(key)
	console.log('已删除存储空间' + storename + '中的' + key + '纪录')
}
复制代码
```

关闭数据库：

```
db.close
复制代码
```

### IndexedDB的优点（相较于前面的存储方案）

- 拥有更大的储存空间
- 能够处理更为复杂和结构化的数据
- 拥有更多的交互控制
- 每个'database'中可以拥有多个'database'和'table'

### IndexedDB的局限性

了解了IndexedDB的优点，我们当然也要来聊一聊IndexedDB的局限性与适用的场景：

#### 1. 存储空间限制

一个单独的数据库项目的大小没有限制。然而可能会限制每个 IndexedDB 数据库的大小。这个限制（以及用户界面对它进行断言的方式）在各个浏览器上也可能有所不同：

- Firefox: 对 IndexedDB 数据库的大小没有限制。在用户界面上只会针对存储超过 50 MB 大小的 BLOB（二进制大对象）请求权限。这个大小的限额可以通过 dom.indexedDB.warningQuota 首选项进行自定义。(定义在 [mxr.mozilla.org/mozilla-cen…](http://mxr.mozilla.org/mozilla-central/source/modules/libpref/src/init/all.js))。
- Google Chrome：[developers.google.com/chrome...ra…](https://developers.google.com/chrome...rage#temporary)

#### 2. 兼容性问题

IndexedDB的兼容来讲比前面所提及的存储方案要差不少，因此在使用IndexedDB时，我们也要好好的考虑兼容性的问题

#### 3. indexedDB受同源策略的限制

indexedDB使用同源原则，这意味着它把存储空间绑定到了创建它的站点的源（典型情况下，就是站点的域或是子域），所以它不能被任何其他源访问。要着重指出的一点是 IndexedDB 不适用于从另一个站点加载进框架的内容 (不管是  还是 。这是一项安全措施。详情请看这个：[bugzilla.mozilla.org/show_bug.cg…](https://bugzilla.mozilla.org/show_bug.cgi?id=595307)



除此之外，IndexedDB还存在诸如：不适合存储敏感数据，相较于web存储机制的操作更加复杂等问题，这都是我们在使用IndexedDB时需要考虑的。

# 防抖与节流

## 函数防抖(debounce)

> 在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。

```javascript
function debounce(func, delay) {
  let timeout;
  return function () {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      func.apply(this, arguments);
    }, delay);
  };
}
```

## 函数节流(throttle)

>  规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。

```js
// 利用时间戳
function throttle(func, frequence = 1000) {
  let timer = Date.now() + frequence;
  return function () {
    if (Date.now() < timer) return;
    func.apply(this, arguments);
    timer = Date.now() + frequence;
  };
}
// 利用定时器
function throttle2(func, frequence = 1000) {
  let timeout = null;
  return function () {
    if (timeout) return;
    timeout = setTimeout(() => {
      func.apply(this, arguments);
      timeout = null;
    }, frequence);
  };
}
```



# Object.defineProperty和Proxy区别

# 脏检查和依赖注入