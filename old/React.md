
# React 核心概念

## JSX 简介

#### 为什么使用 JSX

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合，比如，在 UI 中需要绑定处理事件、在某些时刻状态发生变化时需要通知到 UI，以及需要在 UI 中展示准备好的数据。

React 并没有采用将标记与逻辑进行分离到不同文件这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现关注点分离。

##### JSX 防止注入攻击

React DOM 在渲染所有输入内容之前，默认会进行转义。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止 XSS（cross-site-scripting, 跨站脚本）攻击。

`JSX会自动删除空格符，换行符,原理可用正则替换`

### 元素渲染

**元素是构成 React 应用的最小砖块。**

特点:

1. 与浏览器的 DOM 元素不同，React 元素是创建开销极小的普通对象。React DOM 会负责更新 DOM 来与 React 元素保持一致。
2. React 只更新它需要更新的部分

```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById("root"));
}

setInterval(tick, 1000);
```

尽管每一秒我们都会新建一个描述整个 UI 树的元素，React DOM 只会更新实际改变了的内容，也就是例子中的文本节点。

### 组件 & Props

> 组件，从概念上类似于 JavaScript 函数。它接受任意的入参（即 “props”），并返回用于描述页面展示内容的 React 元素。

#### 渲染组件

当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）以及子组件（children）转换为单个对象传递给组件，这个对象被称之为 “props”。

#### Props 的只读性

组件无论是使用函数声明还是通过 class 声明，都决不能修改自身的 props。
所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。

### State & 生命周期

##### State

State 与 props 类似，但是 state 是私有的，并且完全受控于当前组件。

##### 正确地使用 State

1. 不要直接修改 State

   - 使用 setState();
   - 构造函数是唯一可以给 this.state 赋值的地方

2. State 的更新可能是异步的

   - 出于性能考虑，React 可能会把多个 setState() 调用合并成一个调用。
   - this.props 和 this.state 可能会异步更新，所以你不要依赖他们的值来更新下一个状态
   - 具体可参考 [这个](href=https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/17)

   ```js
   // 正确的访问方法
   this.setState((state, props) => ({
     // 这里相当于回调,当state更新完后,这里的回调才会执行,且state也是最新的
     counter: state.counter + props.increment,
   }));
   ```

3. State 的更新会被合并

   - 当你调用 setState() 的时候，React 会把你提供的对象合并到当前的 state。
   - 这里的合并是浅合并

##### 生命周期

具体参考[React.Component](https://zh-hans.reactjs.org/docs/react-component.html#the-component-lifecycle)

##### 挂载

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- constructor()
- static getDerivedStateFromProps()
- render()
- componentDidMount()

##### 更新

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

- static getDerivedStateFromProps()
- shouldComponentUpdate()
- render()
- getSnapshotBeforeUpdate()  
  - 另外值得一题的是，这个函数的出现，是因为接下来的React 17的异步渲染而准备的。
  - getSnapshotBeforeUpdate 方法**在 React 对视图做出实际改动（如 DOM 更新）发生前被调用，返回值将作为 componentDidUpdate 的第**三个参数。
- componentDidUpdate()

##### 卸载

当组件从 DOM 中移除时会调用如下方法：

- componentWillUnmount()

##### 错误处理

当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：

- static getDerivedStateFromError()
- componentDidCatch()

##### 数据是向下流动的

不管是父组件或是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是 class 组件。

这就是为什么称 state 为局部的或是封装的的原因。除了拥有并设置了它的组件，其他组件都无法访问。

组件可以选择把它的 state 作为 props 向下传递到它的子组件中：

### 事件处理

- 在 JavaScript 中，class 的方法默认不会绑定 this

```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isToggleOn: true };
    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState((state) => ({
      isToggleOn: !state.isToggleOn,
    }));
  }
  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? "ON" : "OFF"}
      </button>
    );
  }
}
ReactDOM.render(<Toggle />, document.getElementById("root"));
```

- 解决方法 1: class fields

```js
class LoggingButton extends React.Component {
  // 此语法确保 `handleClick` 内的 `this` 已被绑定。
  // 注意: 这是 *实验性* 语法。
  handleClick = () => {
    console.log("this is:", this);
  };
  render() {
    return <button onClick={this.handleClick}>Click me</button>;
  }
}
```

- 解决方法 2: 使用箭头函数

`此语法问题在于每次渲染 LoggingButton 时都会创建不同的回调函数。在大多数情况下，这没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染。我们通常建议在构造器中绑定或使用 class fields 语法来避免这类性能问题。`

```js
class LoggingButton extends React.Component {
  handleClick() {
    console.log("this is:", this);
  }

  render() {
    // 此语法确保 `handleClick` 内的 `this` 已被绑定。
    return <button onClick={() => this.handleClick()}>Click me</button>;
  }
}
```

### 条件渲染

- 与运算符 &&
- 三目运算符

### 列表 & Key

##### key

- 如果列表项目的顺序可能会变化，我们不建议使用索引来用作 key 值，因为这样做会导致性能变差，还可能引起组件状态的问题。

- key 只是在兄弟节点之间必须唯一

### 表单

##### 受控组件

在 HTML 中，表单元素（如 `<input>`、` <textarea>` 和 `<select>`）通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 setState()来更新。

我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

##### 处理多个输入

这里使用了 ES6 计算属性名称的语法更新给定输入名称对应的 state 值：

```js
  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }
```

##### 受控输入空值

在受控组件上指定 value 的 prop 会阻止用户更改输入。如果你指定了 value，但输入仍可编辑，则可能是你意外地将 value 设置为 undefined 或 null。

>  **也就是说,当 value 为 undefined 或 null 时,input 是可编辑的,为字符串时,是不可编辑的,需要通过输入事件来更新 state**

### 状态提升

> 通常，多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到最近的共同父组件中去。让我们看看它是如何运作的。

- 在 React 应用中，任何可变数据应当只有一个相对应的唯一“数据源”。通常，state 都是首先添加到需要渲染数据的组件中去。然后，如果其他组件也需要这个 state，那么你可以将它提升至这些组件的最近共同父组件中。你应当依靠自上而下的数据流，而不是尝试在不同组件间同步 state。
- 虽然提升 state 方式比双向绑定方式需要编写更多的“样板”代码，但带来的好处是，排查和隔离 bug 所需的工作量将会变少。
- 由于“存在”于组件中的任何 state，仅有组件自己能够修改它，因此 bug 的排查范围被大大缩减了。此外，你也可以使用自定义逻辑来拒绝或转换用户的输入。

### 组合 VS 继承

> 我们推荐使用组合而非继承来实现组件间的代码重用。

- 插槽的形式

```js
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">{props.left}</div>
      <div className="SplitPane-right">{props.right}</div>
    </div>
  );
}

function App() {
  return <SplitPane left={<Contacts />} right={<Chat />} />;
}
```

# 高阶组件

### 作用

- 复用的高级技巧,减少相同代码的冗余

### 高阶组件定义

> a higher-order component is a function that takes a component and returns a new component.

翻译：高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件。

理解了吗？看了定义似懂非懂？继续往下看。

### 函数模拟高阶组件

我们通过普通函数来理解什么是高阶组件哦~

最普通的方法，一个 welcome，一个 goodbye。两个函数先从 localStorage 读取了 username，然后对 username 做了一些处理。

```js
function welcome() {
  let username = localStorage.getItem("username");
  console.log("welcome " + username);
}
function goodbey() {
  let username = localStorage.getItem("username");
  console.log("goodbey " + username);
}

welcome();
goodbey();
```

我们发现两个函数有一句代码是一样的，这叫冗余唉。不好不好~（你可以把那一句代码理解成平时的一大堆代码）

我们要写一个中间函数，读取 username,他来负责把 username 传递给两个函数。

```js
function welcome(username) {
  console.log("welcome " + username);
}
function goodbey(username) {
  console.log("goodbey " + username);
}
function wrapWithUsername(wrappedFunc) {
  let newFunc = () => {
    let username = localStorage.getItem("username");
    wrappedFunc(username);
  };
  return newFunc;
}
welcome = wrapWithUsername(welcome);
goodbey = wrapWithUsername(goodbey);
welcome();
goodbey();
```

好了，我们里面的`wrapWithUsername`函数就是一个“高阶函数”。
他做了什么？他帮我们处理了 username，传递给目标函数。我们调用最终的函数 welcome 的时候，根本不用关心 username 是怎么来的。

我们增加个用户 study 函数。

```js
function study(username) {
  console.log(username + " study");
}
study = wrapWithUsername(study);

study();
```

这里你是不是理解了为什么说 wrapWithUsername 是高阶函数？我们只需要知道，用 wrapWithUsername 包装我们的 study 函数后，study 函数第一个参数是 username。

我们写平时写代码的时候，不用关心 wrapWithUsername 内部是如何实现的。

### 高阶组件

高阶组件就是一个没有副作用的纯函数。

我们把上一节的函数统统改成 react 组件。

最普通的组件哦。
welcome 函数转为 react 组件。

```js
import React, { Component } from "react";
class Welcome extends Component {
  constructor(props) {
    super(props);
    this.state = {
      username: "",
    };
  }
  componentWillMount() {
    let username = localStorage.getItem("username");
    this.setState({
      username: username,
    });
  }
  render() {
    return <div>welcome {this.state.username}</div>;
  }
}
export default Welcome;
```

goodbey 函数转为 react 组件。

```js
import React, { Component } from "react";
class Goodbye extends Component {
  constructor(props) {
    super(props);
    this.state = {
      username: "",
    };
  }
  componentWillMount() {
    let username = localStorage.getItem("username");
    this.setState({
      username: username,
    });
  }
  render() {
    return <div>goodbye {this.state.username}</div>;
  }
}
export default Goodbye;
```

现在你是不是更能看到问题所在了？两个组件大部分代码都是重复的唉。
按照上一节 wrapWithUsername 函数的思路，我们来写一个高阶组件(高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件)。

```js
import React, { Component } from "react";
export default (WrappedComponent) => {
  class NewComponent extends Component {
    constructor() {
      super();
      this.state = {
        username: "",
      };
    }
    componentWillMount() {
      let username = localStorage.getItem("username");
      this.setState({
        username: username,
      });
    }
    render() {
      return <WrappedComponent username={this.state.username} />;
    }
  }
  return NewComponent;
};
```

这样我们就能简化 Welcome 组件和 Goodbye 组件。

```js
import React, { Component } from "react";
import wrapWithUsername from "wrapWithUsername";
class Welcome extends Component {
  render() {
    return <div>welcome {this.props.username}</div>;
  }
}
Welcome = wrapWithUsername(Welcome);
export default Welcome;
import React, { Component } from "react";
import wrapWithUsername from "wrapWithUsername";

class Goodbye extends Component {
  render() {
    return <div>goodbye {this.props.username}</div>;
  }
}
Goodbye = wrapWithUsername(Goodbye);
export default Goodbye;
```

看到没有，高阶组件就是把 username 通过 props 传递给目标组件了。目标组件只管从 props 里面拿来用就好了。

> 到这里位置，高阶组件就讲完了。你再返回去理解下定义，是不是豁然开朗~

你现在理解 react-redux 的 connect 函数~

把 redux 的 state 和 action 创建函数，通过 props 注入给了 Component。
你在目标组件 Component 里面可以直接用 this.props 去调用 redux state 和 action 创建函数了。

```js
ConnectedComment = connect(mapStateToProps, mapDispatchToProps)(Component);
```

相当于这样

```js
// connect 是一个返回函数的函数（就是个高阶函数）
const enhance = connect(mapStateToProps, mapDispatchToProps);
// 返回的函数就是一个高阶组件，该高阶组件返回一个与 Redux store
// 关联起来的新组件
const ConnectedComment = enhance(Component);
antd 的 Form 也是一样的

const WrappedNormalLoginForm = Form.create()(NormalLoginForm);
```

参考文章

- [助你完全理解React高阶组件](https://github.com/i-want-offer/FE-Interview-questions/blob/master/%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96/React%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96.md))

# React VS Vue 以及各自的特点 优缺点

## React

### 优点

- 引入了虚拟 DOM 和 diff 算法，提高了 Web 性能

`VDOM` :通过 JS 的对象模拟出 dom 的节点,然后通过 render 方法将虚拟 dom 转换成真实的 dom

算法复杂度为 O(n)
虚拟 dom 如何计算 diff
key 属性的作用

`Diff算法`:根据两个虚拟节点比较,返回一个 patch 对象,完成页面重新渲染

差异计算

先序深度优先遍历

## 为什么更快了

当然如果真的这样大面积的操作 DOM，性能会是一个很大的问题，所以 React 实现了一个虚拟 DOM，组件 DOM 结构就是映射到这个虚拟 DOM 上，React 在这个虚拟 DOM 上实现了一个 diff 算法，当要更新组件的时候，会通过 diff 寻找到要变更的 DOM 节点，再把这个修改更新到浏览器实际的 DOM 节点上，所以实际上不是真的渲染整个 DOM 树。这个虚拟 DOM 是一个纯粹的 JS 数据结构，所以性能会比原生 DOM 快很多。

- 组件化，模块化开发。便于我们后期维护性
- 单向数据流，比较有序，便于数据管理。

## 缺点

- react 中只是 MVC 模式的 View 部分，所以在 react 开发中要依赖很多其他模块。

- 当父组件进行重新渲染时，即使子组件的 props 或 state 没有任何改变，也会同样进行重新渲染。（react 如何避免重复渲染）

## 特点

1. 声明式设计（React 采用声明式，可以轻松描述应用。）
2. 高性能（React 通过对 DOM 的模拟，最大限度地减少与 DOM 的交互。）
3. 灵活（React 可以与已知的库或框架很好地配合。）

## 在 React 如何做性能优化？

1. 给 DOM 的遍历上加上唯一的 key。提高 diff 算法效率。
   尽量不要用 index,如果说在 DOM 中删了某一个节点，index 也就会发生变化，这时候就会重新渲染，所以 key 值最好使用 id。
2. 能用 const 声明的就用 const。在 render 里面尽量减少新变量的创建以及函数的指向改变问题。
3. 减少对真实 DOM 的操作。
4. 如果是用 webpack 搭建环境的话，当一个包过大加载过慢时，可分成多个包来优化。
5. 使用 react-loadable，按需加载路由
6. 定时器，超时器使用过后在 unmount 中清除。
7. immutable 对象管理状态 让状态不能被更改
8. 把 component 更换成 pureComponent 也可以进行优化

## 你对 pureComponent 的理解

1. 当更新时，如果组件的 props 或者 state 都没有改变，render 函数就不会触发。
2. 省去虚拟 DOM 的生成和对比过程，达到提升性能的目的。
3. 具体原因是因为 react 自动帮我们做了一层浅比较

## react 如何避免重复渲染

React 官方提供了 PureRenderMixin 插件，它的功能就是在不需要重新渲染的情况下让 shouldComponentUpdate 返回 false, 使用这个插件就能够减少不必要的重新渲染，性能得到也得到一些提升。

但是在 React 的新版本里面，提供了 React.PureComponent，而不需要使用这个插件。 所以说一个较大的组件决定重渲染的时候，我们可以在每一个子组件中绑定（新的）shouldComponentUpdate 方法，这样可以减少子组件重新渲染的次数。

## 无状态组件和有状态组件有什么区别

**无状态组件**

> 无状态组件的创建形式使代码的可读性更好，而且减少了大量冗余的代码，以至于只有一个 render 方法。组件不会被实例化组件不能访问 this 对象，也不能访问生命周期方法无状态组件只能访问输入的 props ，同样的 props 渲染同样的结果，而且没有副作用。

**有状态组件**

> React.Component 创建的组件，其成员函数不会自动绑定 this，如果我们没有手动绑定 this，就不能获取当前组件实例对象。React.Component 创建的组件，其状态 state 是在 constructor 中像初始化组件属性一样去声明。

`补充：React.Component 有三种手动绑定方法`

```javascript
constructor(props) {
  super(props);
  this.handleClick = this.handleClick.bind(this); //构造函数中绑定
}
```

```jsx
<div onClick={this.handleClick.bind(this)}></div> //使用 bind 来绑定
```

```jsx
<div onClick={() => this.handleClick()}></div> //使用 arrow function 来绑定
```

如何选择哪种方式创建组件只要有可能，尽量使用无状态组件。如果需要 state、生命周期方法等，使用 class 的创建组件

#### react 在哪个生命周期做优化?

可以在 shouldComponentUpdate 这个方法进行优化，用来判断是否调用 render 方法重绘 dom。
因为 dom 的重绘非常消耗性能，所以可以在这个方法中去做 dom 的 diff 算法的优化，这样就可以极大的提高性能。

但对于我们中级工程师来说，一般我不会去修改 shouldComponentUpdate 这个方法。而是直接去使用。True/false

#### react 组件间传值

- props 传值，父组件通过 props 向子组件传值，子组件通过回调函数向父组件传值
- redux 跨级通信。
- 通过 prop-types 的 context 实现跨级通信
  跨级组件间双向通信：使用 context 对象，根组件和其他所有子孙通信，不太适合组件间通信（可以实现，不好维护）
- 使用事件订阅实现非嵌套组件间通信，也可以实现跨级组件间通信

#### porps 和 state 的区别

`props`:组件中通讯使用,自上而下传递,只读
`state`:组件内的状态维护,可以更新子组件的 props

#### 对 react 中事件机制的理解

Reac 事件是合成事件，不是原生事件。

合成事件和原生事件的区别：

（1） 写法不同：合成事件是 camal 命名法，原生事件是全部小写

（2） 绑定位置不同：合成事件全部委派到 document 上，原生事件绑定到真实 dom 上。所以一般是在 componentDidMount 中或者 ref 回调函数中绑定，在 componentWillUnmount 阶段进行解绑，防止内存泄漏。

（3） 执行顺序不同：先执行原生事件，事件冒泡至 document 上，再执行合成事件。

#### react 在哪个生命周期做优化

可以在 shouldComponentUpdate 这个方法进行优化，用来判断是否调用 render 方法重绘 dom。
因为 dom 的重绘非常消耗性能，所以可以在这个方法中去做 dom 的 diff 算法的优化，这样就可以极大的提高性能。

但对于我们中级工程师来说，一般我不会去修改 shouldComponentUpdate 这个方法。而是直接去使用。True/false





# 关于setState

React的setState本身并不是异步的，是因为其批处理机制给人一种异步的假象。

【React的更新机制】

生命周期函数和合成事件中：

1. 无论调用多少次setState，都不会立即执行更新。而是将要更新的state存入'_pendingStateQuene',将要更新的组件存入'dirtyComponent';
2. 当根组件didMount后，批处理机制更新为false。此时再取出'_pendingStateQuene'和'dirtyComponent'中的state和组件进行合并更新；

原生事件和异步代码中：

1. 原生事件不会触发react的批处理机制，因而调用setState会直接更新；
2. 异步代码中调用setState，由于js的异步处理机制，异步代码会暂存，等待同步代码执行完毕再执行，此时react的批处理机制已经结束，因而直接更新。

总结：
react会表现出同步和异步的现象，但本质上是同步的，是其批处理机制造成了一种异步的假象。（其实完全可以在开发过程中，在合成事件和生命周期函数里，完全可以将其视为异步）

# HOC & Render Props & Hooks

## HOC

![img](https://pic.4sus2.com/uPic/1602122918916sn8ae5.png)

高阶组件其实并不是组件，只是一个函数而已。它接收一个组件作为参数，返回一个新的组件。我们可以在新的组件中做一些功能增加，渲染原有的组件。这样返回的组件增强了功能，但渲染与原有保持一致，没有破坏原有组件的逻辑。

因此在提取不同类别组件相似的行为时，高阶组件是非常合适的选择。举例说明的话，组件异步加载、异步加载 script 后显示组件、数据源绑定、拖拽排序。

## render props

![img](https://pic.4sus2.com/uPic/1602122918913G4kgjZ.png)

去渲染一个父子组件，但是子组件依赖于父组件的某些数据: 可以在父组件中做一些通用性的逻辑，并将数据抛给子组件。子组件可以任意渲染成自己想要的样子。比如说我们可以在父组件中做一个倒计时的逻辑，然后把倒计时的时间传给子组件，这样子组件任意渲染成什么样都可以，子组件只自己知道会定时的拿到新的时间而已。

## Hooks

react hooks 的意义是：

- 代替 render props 这种 HOC 方式复用逻辑
- 代替生命周期函数，不再将逻辑散落在生命周期函数里
- 更加函数式，没有 class 也就少了副作用

看到 useEffect ，我想了想我对于副作用的理解：`UI = F(Props)` ，一个组件最终的dom结构与样式是由父级传递的props决定的。

「纯函数」:意思是固定的输入必然有固定的输出，它不依赖任何外部因素，也不会对外部环境产生影响。

react希望自己的组件渲染也是个纯函数，所以有了纯函数组件。然而真正的业务场景是有各种状态的，实际影响UI的还有内部的state。(其实还有context，暂时先不讨论）。

UI = F(props, state, context)

这个state可能会因为各种原因产生变化，从而导致组件的渲染结果不一致。相同的入参（props）下，每次render都有可能返回不同的UI。因此任何导致此现象的行为都是副作用（side effects）。比如用户点击下一页，导致页码与列表发生变化，这就是副作用。同样的props，不点击时是第一页数据，点击一下后，变成了第二页的数据or请求失败的页面or其他UI交互。

当然state是明面上影响了UI，暗地里，可能还有其他因素会影响UI。比如比如网络请求、操作 DOM等。

## hooks vs class

现在 hooks 让函数式组件完全可以代替 class 组件，用了 hooks 的函数组件可以很明显地看到两部分：**数据处理和 UI**，相比 class 组件中的混乱声明周期更高明一点。

> Hooks可以完全代替class 和HOC

![image](https://pic.4sus2.com/uPic/1602122918914rHOKX1.png)

# React & Vue

#### 相同点

1. 都有组件开发和V-DOM
2. 都支持props进行父子组件数据通讯
3. 都是数据驱动,不直接操作真实dom,更新状态数据界面就自动更新
4. 都支持SSR
5. 都有native的方案,reactnative,weex

#### 不同点

1. 数据绑定:
   - vue是双向绑定(界面=>数据/数据=>界面)
   - react是单向数据流(数据=>界面)
2. 组件写法不一样
   - react推荐jsx
   - vue把所有东西写在模板里
3. 状态变化
   - react必须使用setState()更新组件状态
   - vue的data属性在vue对象中管理
4. VDOM实现不完全一样
   - vue会追踪所有依赖,不需要重新渲染整个组件树
5. React是MVC的View层,vue是MVVM的模式



# React 中 keys 的作用是什么？

在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。

- 在 React Diff 算法中 React 会借助元素的 Key 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染。
- 此外，React 还需要借助 Key 值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中 Key 的重要性。

# 调用 setState 之后发生了什么？

- 在代码中调用 setState 函数之后，React 会将传入的参数对象与组件当前的状态合并，然后触发所谓的调和过程（Reconciliation）。

- 经过调和过程，React 会以相对高效的方式根据新的状态构建 React 元素树并且着手重新渲染整个 UI 界面。在 React 得到元素树之后，React 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。
- 在差异计算算法中，React 能够相对精确地知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。

# shouldComponentUpdate 是做什么的

shouldComponentUpdate 这个方法用来判断是否需要调用 render 方法重新描绘 dom。因为 dom 的描绘非常消耗性能，如果我们能在 shouldComponentUpdate 方法中能够写出更优化的 dom diff 算法，可以极大的提高性能。

# 为什么虚拟 dom 会提高性能?(必考)

虚拟 dom 相当于在 js 和真实 dom 中间加了一个缓存，利用 dom diff 算法避免了没有必要的 dom 操作，从而提高性能。

用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异把 2 所记录的差异应用到步骤 1 所构建的真正的 DOM 树上，视图就更新了。

参考 [如何理解虚拟 DOM?-zhihu](https://www.zhihu.com/question/29504639?sort=created)

# react diff 原理（常考）

- 把树形结构按照层级分解，只比较同级元素。
- 给列表结构的每个单元添加唯一的 key 属性，方便比较。
- React 只会匹配相同 class 的 component（这里面的 class 指的是组件的名字）
- 合并操作，调用 component 的 setState 方法的时候, React 将其标记为 dirty.到每一个事件循环结束, React 检查所有标记 dirty 的 component 重新绘制.
- 选择性子树渲染。开发人员可以重写 shouldComponentUpdate 提高 diff 的性能。

参考：React 的 diff 算法

## Diff

React Diff 会帮助我们计算出 Virtual DOM 中*真正发生变化的部分*，并且只针对该部分进行实际的DOM操作，而不是对整个页面进行重新渲染

### React diff算法策略

- 针对树结构(tree diff)：对UI层的DOM节点跨层级的操作进行忽略。（数量少）
- 针对组件结构(component diff)：拥有相同*类*的两个组件生成相似的树形结构，拥有不同*类*的两个组件会生成不同的属性结构。
- 针对元素结构(element-diff): 对于同一层级的一组节点，使用具有*唯一性*的id区分 (key属性)

#### tree diff 的特点

- React 通过使用 updateDepth 对 虚拟DOM树进行层次遍历
- 两棵树只对同一层级节点进行比较，只要该节点不存在了，那么该节点与其所有子节点会被*完全删除*,不在进行进一步比较。
- 只需要遍历一次，便完成对整个DOM树的比较。

> React diff 只考虑*同层次*的节点位置变换,若为跨层级的位置变化，则是创建节点和删除节点的操作。即在新位置上重新创建相同的节点，而删除原位置的节点。

Tips:  React 官方建议不要进行DOM节点的跨层级操作，可是通过CSS来隐藏，显示节点，而不是真正地删除和添加DOM节点，保持稳定的DOM结构会对性能提升有帮助。

#### component diff的特点

- 同一类型的组件，按照原策略(tree diff)比较 virtual DOM tree
- 同类型组件，组件A转化为了组件B，如果virtual DOM 无变化，可以通过`shouldComponentUpdate()`方法来判断是否

> [React官方文档对于 shouldComponentUpdate的介绍](https://zh-hans.reactjs.org/docs/optimizing-performance.html#shouldcomponentupdate-in-action)

- 不同类型的组件，那么diff算法会把要改变的组件判断为`dirty component`,从而替换整个组件的所有节点。

> 就算结构再相似的组件，只要 React 判断是不同的组件，就不会判断是否为不同类型的组件，就不会比较其结构，而是删除组件以及其子组件，并创建新的组件以及其子节点。

#### element diff特点

对于处于同一层级的节点，React diff 提供了三种节点操作: 插入，移动，删除

- 插入： 新的组件不在原来的集合中，而是全新的节点，则对集合进行插入操作。
- 删除：组件已经在集合中，但集合已经更新，此时节点就需要删除。
- 移动：组件*已经存在于*集合中，并且集合更新时，组件并没有发生更新，只是位置发生改变，例如：(A,B,C,D) → (A,D,B,C), 如果为*传统diff*则会在检测到旧集合中第二位为B，新集合第二位为D时删除B，插入D，并且后面的所有节点都要重新加载，而 React diff 则是通过向同一层的节点添加 *唯一key* 进行区分，并且移动。

#### 一些移动的场景与逻辑

##### 节点相同，位置不同

![diff 1](https://pic.4sus2.com/uPic/1602122918913nUbqX5.jpg)

按新集合中顺序开始遍历

1. B在新集合中 lastIndex(类似浮标) = 0, 在旧集合中 index = 1，index > `lastIndex` 就认为 B 对于集合中其他元素位置无影响，不进行移动，之后`lastIndex` = max(index, lastIndex) = 1
2. A在旧集合中 index = 0， 此时 `lastIndex` = 1, 满足 index < `lastIndex`, 则对A进行移动操作，此时`lastIndex` = max(Index, lastIndex) = 1
3. D和B操作相同，同(1)，不进行移动，此时`lastIndex`=max(index, lastIndex) = 3
4. C和A操作相同，同(2)，进行移动，此时`lastIndex` = max(index, lastIndex) = 3

##### 节点位置均有变化

![diff 2](https://pic.4sus2.com/uPic/1602122918913ayT278.jpg)

1.同上面那种情形，B不进行移动，`lastIndex`=1

2.新集合中取得E,发现旧中不存在E，在 `lastIndex`处创建E，`lastIndex`++

3.在旧集合中取到C，C不移动，`lastIndex`=2

4.在旧集合中取到A，A移动到新集合中的位置，`lastIndex`=2

5.完成新集合中所有节点diff后，对旧集合进行循环遍历，寻找新集合中不存在但就集合中的节点(此例中为D)，删除D节点。

#### React diff 的不足之处

![img](https://pic.4sus2.com/uPic/1602122918913e04p6d.jpeg)

此例中D直接从最后一位提升至第一位，导致`lastIndex`在第一步直接提升为3，使ABC在进行index与lastIndex的判断时均处于 index < lastIndex 的情况，使ABC都需要做移动操作。所以我们应该减少将*最后一个*节点提升至*第一个*的操作，如果操作频率较大或者节点数量较多时，会对渲染性能产生影响。

### 小结

- React diff 与 传统diff 的不同是 React通过优化将O(n^3)提升至O(n)
- React 通过三个方面对tree diff, component diff, element diff 进行了优化
- 在开发时，尽量保持稳定的DOM结构，并且减少将最后的节点移动到首部的操作，能够优化渲染性能。






# React 中 refs 的作用是什么？

Refs 是 React 提供给我们的安全访问 DOM 元素或者某个组件实例的句柄。

我们可以为元素添加 ref 属性然后在回调函数中接受该元素在 DOM 树中的句柄，该值会作为回调函数的第一个参数返回

# (在构造函数中)调用 super(props) 的目的是什么

在 super() 被调用之前，子类是不能使用 this 的，在 ES2015 中，子类必须在 constructor 中调用 super()。传递 props 给 super() 的原因则是便于(在子类中)能在 constructor 访问 this.props。

# 了解 redux 么，说一下 redux 把

- redux 是一个应用数据流框架，主要是解决了组件间状态共享的问题，原理是集中式管理，主要有三个核心方法，action，store，reducer，
- 工作流程是 view 调用 store 的 dispatch 接收 action 传入 store，reducer 进行 state 操作，view 通过 store 提供的 getState 获取最新的数据，
- flux 也是用来进行数据操作的，有四个组成部分 action，dispatch，view，store，工作流程是 view 发出一个 action，派发器接收 action，让 store 进行数据更新，更新完成以后 store 发出 change，view 接受 change 更新视图。Redux 和 Flux 很像。主要区别在于 Flux 有多个可以改变应用状态的 store，在 Flux 中 dispatcher 被用来传递数据到注册的回调事件，但是在 redux 中只能定义一个可更新状态的 store，redux 把 store 和 Dispatcher 合并,结构更加简单清晰
- 新增 state,对状态的管理更加明确，通过 redux，流程更加规范了，减少手动编码量，提高了编码效率，同时缺点时当数据更新时有时候组件不需要，但是也要重新绘制，有些影响效率。一般情况下，我们在构建多交互，多数据流的复杂项目应用时才会使用它们

## redux 有什么缺点

- 一个组件所需要的数据，必须由父组件传过来，而不能像 flux 中直接从 store 取。
- 当一个组件相关数据更新时，即使父组件不需要用到这个组件，父组件还是会重新 render，可能会有效率影响，或者需要写复杂的 shouldComponentUpdate 进行判断。

# React性能优化

[链接](https://github.com/i-want-offer/FE-Interview-questions/blob/master/%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96/React%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96.md)