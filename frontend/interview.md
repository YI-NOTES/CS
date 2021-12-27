<!--
 * Date: 2021-12-27 19:17:11
 * Author: Zeng Yi
 * LastEditTime: 2021-12-27 22:05:14
 * LastEditors: Zeng yi
 * Description: //TODO
-->

# CSS

## 1. 让一个元素水平垂直居中，可以用几种方案实现？

**水平居中**

- 对于行内元素： text-align: center;
- 对于确定宽度的块级元素：  
    （1）width和margin实现。margin: 0 auto;  
    （2）绝对定位和margin-left: -width/2, 前提是父元素position: relative
- 对于宽度未知的块级元素  
    （1）table标签配合margin左右auto实现水平居中。使用table标签（或直接将块级元素设值为display:table），再通过给该标签添加左右margin为auto。  
    （2）inline-block实现水平居中方法。display：inline-block和text-align:center实现水平居中。  
    （3）绝对定位+transform，translateX可以移动本身元素的50%。  
    （4）flex布局使用justify-content:center

**垂直居中**

1. 利用 line-height 实现居中，这种方法适合纯文字类
2. 通过设置父容器 相对定位 ，子级设置 绝对定位 ，标签通过margin实现自适应居中
3. 弹性布局 flex :父级设置display: flex; 子级设置margin为auto实现自适应居中
4. 父级设置相对定位，子级设置绝对定位，并且通过位移 transform 实现
5. table 布局，父级通过转换成表格形式，然后子级设置 vertical-align 实现。（需要注意的是：vertical-align: middle使用的前提条件是内联元素以及display值为table-cell的元素）。

## 2. flex, 左右300宽，中间宽度自动延伸应该怎么做？

``` html
<!DOCTYPE html>
<html>
<title>Web Page Design</title>
<head>
<style>
.container{
    display:flex;
}
.box1{
    width:100px;
    background-color:red;
    height:200px;
}
.box2{
    flex:1;
    background-color: blue;
}
.box3{
    width:100px;
    background-color: yellow;
}
</style>
</head>
<body>
<div class="container">
<div class="box1">Box1</div>
<div class="box2">Box2</div>
<div class="box3">Box3</div>
</div>    
</body>
</html>
```

## 3. 什么是BFC？

`BFC` 全称：`Block Formatting Context`， 名为 "块级格式化上下文"。

`W3C`官方解释为：`BFC`它决定了元素如何对其内容进行定位，以及与其它元素的关系和相互作用，当涉及到可视化布局时，`Block Formatting Context`提供了一个环境，`HTML`在这个环境中按照一定的规则进行布局。

简单来说就是，`BFC`是一个完全独立的空间（布局环境），让空间里的子元素不会影响到外面的布局。那么怎么使用`BFC`呢，`BFC`可以看做是一个`CSS`元素属性

这里简单列举几个触发`BFC`使用的`CSS`属性

- overflow: hidden
- display: inline-block
- position: absolute
- position: fixed
- display: table-cell
- display: flex

1.使用Float脱离文档流，高度塌陷

2.Margin边距重叠

# JS

## 原型链是什么

JavaScript对象通过 proto 指向父类对象，直到指向 Object 对象为止，这样就形成了一个原型指向的链条， 即原型链。

对象的 hasOwnProperty() 来检查对象自身中是否含有该属性。

使用 in 检查对象中是否含有某个属性时，如果对象中没有但是原型链中有，也会返回 true。

### 防抖与节流

(1).防抖：用户触发事件过于频繁时，只要最后一次事件的操作

(2).节流：控制执行次数

### 事件循环

process.nextTick() setImmediate 方法

process.nextTick() 同步代码执行之后，异步代码执行之前运行（callback）

setImmediate() 异步代码执行之后执行 => 当前事件循环结束后执行

eventLoop：代码运行时，同步代码会直接放入运行栈，异步代码会放入任务队列中执行，将回调放入消息队列，当调用栈没有工作，也就是说同步代码已执行完毕，eventLoop 发挥作用，只做1件事情负责监听调用栈和消息队列，当调用栈没有任务，event 将消息队列中的异步任务放入调用栈执行

宏任务、微任务：宏：计时器、ajax、读取文件；微：promise.then （new Promise（同步）、then（异步））

同步=> process.nextTick() => 微任务 => 宏任务 => setImmediate

# ES6

## symbol()

`Symbol` 是 `ES6` 新推出的一种基本类型，它表示独一无二的值

它可以选择接受一个字符串作为参数或者不传，但是相同参数的两个`Symbol` 值不相等

```js
//不传参数
const s1 = Symbol();
const s2 = Symbol();
console.log(s1 === s2); // false

// 传入参数
const s3 = Symbol('debug');
const s4 = Symbol('debug');
console.log(s3 === s4); // false
复制代码
```

可以通过`typeof`判断是否为`Symbol`类型

```
console.log(typeof s1); // symbol
复制代码
```

不是一个完整的构造函数，不能通过`new Symbol()` 来创建

```js
const s1 = new Symbol();
// Uncaught TypeError: Symbol is not a constructor
复制代码
```

`Symbol`有两个方法，如下：

- Symbol.for()
- Symbol.keyFor()

Symbol.for()

用于将描述相同的`Symbol`变量指向同一个`Symbol`值

```js
let a1 = Symbol.for('a');
let a2 = Symbol.for('a');
a1 === a2  // true
typeof a1  // "symbol"
typeof a2  // "symbol"

let a3= Symbol("a");
a1 === a3      // false


```

它跟`symbol()`的区别是`Symbol()`定义的值每次都是新建，即使描述相同值也不相等

而`Symbol.for()`定义的值会先检查给定的描述是否已经存在，如果不存在才会新建一个值，否则描述相同则他们就是同一个值

Symbol.keyFor()

`Symbol.keyFor()`是用来检测该字符串参数作为名称的 `Symbol` 值是否已被登记，返回一个已登记的 `Symbol` 类型值的 `key`

```js
let a1 = Symbol.for("a");
Symbol.keyFor(a1);    // "a"

let a2 = Symbol("a");
Symbol.keyFor(a2);    // undefined


```

`Symbol`只有一个属性：

- description

description

用来返回`Symbol`数据的描述

## async / await

一个函数如果加上 async ，那么该函数就会返回一个 Promise。

await 只能在 async 函数中使用，可以把 async 看成将函数返回值使用 Promise.resolve() 包裹了下。

async 和 await 相比直接使用 Promise 来说，优势在于处理 then 的调用链，能够更清晰准确的写出代码。缺点在于滥用 await 可能会导致性能问题，因为 await 会阻塞代码，也许之后的异步代码并不依赖于前者，但仍然需要等待前者完成，导致代码失去了并发性。

# Typescript

**1. 使用 TypeScript 有什么好处？**

- TypeScript 更具表现力，这意味着它的语法混乱更少。
- 由于高级调试器专注于在编译时之前捕获逻辑错误，因此调试很容易。
- 静态类型使 TypeScript 比 JavaScript 的动态类型更易于阅读和结构化。
- 由于通用的转译，它可以跨平台使用，在客户端和服务器端项目中。

**2. TypeScript中的类型**

类型系统表示语言支持的不同类型的值。它在程序存储或操作所提供的值之前检查其有效性。

它可以分为两种类型，
内置：包括数字(number)，字符串(string)，布尔值(boolean)，无效(void)，空值(null)和未定义(undefined)。

用户定义的：它包括枚举(enums)，类(classes)，接口(interfaces)，数组(arrays)和元组(tuple)。

**3. TypeScript有哪些面向对象的关键字**

TypeScript支持以下面向对象的术语：
模块（Modules）
类（Classes）
接口（Interfaces）
继承（Inheritance）
数据类型（Data Types）
成员函数（Member functions）

# React

## React中的状态是什么？它是如何使用的？State是同步的还是异步的？setState 什么时候同步什么时候异步？

- setState 只在合成事件（react为了解决跨平台，兼容性问题，自己封装了一套事件机制，代理了原生的事件，像在jsx中常见的onClick、onChange这些都是合成事件）和钩子函数（生命周期）中是“异步”的，在原生事件和 setTimeout 中都是同步的
- setState的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形式了所谓的“异步”，当然可以通过第二个参数 setState(partialState, callback) 中的callback拿到更新后的结果
- setState 的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout 中不会批量更新，在“异步”中如果对同一个值进行多次 setState ， setState 的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时 setState 多个不同的值，在更新时会对其进行合并批量更新

## Fiber 如何解决问题的

Fiber 把一个渲染任务分解为多个渲染任务，而不是一次性完成，把每一个分割得很细的任务视作一个"执行单元"，React 就会检查现在还剩多少时间，如果没有时间就将控制权让出去，故任务会被分散到多个帧里面，中间可以返回至主进程控制执行其他任务，最终实现更流畅的用户体验。

即是实现了"增量渲染"，实现了可中断与恢复，恢复后也可以复用之前的中间状态，并给不同的任务赋予不同的优先级，其中每个任务更新单元为 React Element 对应的 Fiber 节点。

## Fiber 实现原理

实现的方式是`requestIdleCallback`这一 API，但 React 团队 polyfill 了这个 API，使其对比原生的浏览器兼容性更好且拓展了特性。

> `window.requestIdleCallback()`方法将在浏览器的空闲时段内调用的函数排队。这使开发者能够在主事件循环上执行后台和低优先级工作，而不会影响延迟关键事件，如动画和输入响应。函数一般会按先进先调用的顺序执行，然而，如果回调函数指定了执行超时时间 timeout，则有可能为了在超时前执行函数而打乱执行顺序。

`requestIdleCallback`回调的执行的前提条件是当前浏览器处于空闲状态。

即`requestIdleCallback`的作用是在浏览器一帧的剩余空闲时间内执行优先度相对较低的任务。首先 React 中任务切割为多个步骤，分批完成。在完成一部分任务之后，将控制权交回给浏览器，让浏览器有时间再进行页面的渲染。等浏览器忙完之后有剩余时间，再继续之前 React 未完成的任务，是一种合作式调度。

简而言之，由浏览器给我们分配执行时间片，我们要按照约定在这个时间内执行完毕，并将控制权还给浏览器。

### 什么是高阶组件（HOC）

高阶组件是重用组件逻辑的高级方法，是一种源于 React 的组件模式。 HOC 是自定义组件，在它之内包含另一个组件。它们可以接受子组件提供的任何动态，但不会修改或复制其输入组件中的任何行为。你可以认为 HOC 是“纯（Pure）”组件。

HOC可用于许多任务，例如：

- 代码重用，逻辑和引导抽象
- 渲染劫持
- 状态抽象和控制
- Props 控制

### 常用的Hooks有哪些？

- `useState()`状态钩子。为函数组建提供内部状态
- `useContext()`共享钩子。该钩子的作用是，在组件之间共享状态。 可以解决react逐层通过Porps传递数据，它接受React.createContext()的返回结果作为参数，使用useContext将不再需要Provider 和 Consumer
- `useReducer()`状态钩子。Action 钩子。useReducer() 提供了状态管理，其基本原理是通过用户在页面中发起action, 从而通过reducer方法来改变state, 从而实现页面和状态的通信。使用很像redux
- `useEffect()`副作用钩子。它接收两个参数， 第一个是进行的异步操作， 第二个是数组，用来给出Effect的依赖项
- `useRef()`获取组件的实例；渲染周期之间共享数据的存储(state不能存储跨渲染周期的数据，因为state的保存会触发组件重渲染）,`useRef传入一个参数initValue，并创建一个对象{ current: initValue }给函数组件使用，在整个生命周期中该对象保持不变`
- `useMemo和useCallback`可缓存函数的引用或值，useMemo缓存数据，useCallback缓存函数，两者是Hooks的常见优化策略，`useCallback(fn,deps)相当于useMemo(()=>fn,deps)`,经常用在下面两种场景:  
    1、要保持引用相等；对于组件内部用到的 object、array、函数等，  
    2、用在了其他 Hook 的依赖数组中，或者作为 props 传递给了下游组件，应该使用 useMemo/useCallback）

### React 加入 Hooks 的意义是什么？为什么 React 要加入Hooks 这一特性？

`为了解决一些component问题：`

1. 组件之间的逻辑状态难以复用
2. 大型复杂的组件很难拆分
3. Class语法的使用不友好

`比如说：`

- 类组件可以访问生命周期，函数组件不能
- 类组件可以定义维护state(状态)，函数组件不可以
- 类组件中可以获取到实例化后的this,并基于这个this做一些操作，而函数组件不可以
- mixins:变量作用于来源不清、属性重名、Mixins引入过多会导致顺序冲突
- HOC和Render props：组件嵌套过多，不易渲染调试、会劫持props,会有漏洞

`有了Hooks:`

- Hooks 就是让你不必写class组件就可以用state和其他的React特性；
- 也可以编写自己的hooks在不同的组件之间复用

`Hooks优点:`

- 没有破坏性改动：完全可选的。 你无需重写任何已有代码就可以在一些组件中尝试 Hook。100% 向后兼容的。 Hook 不包含任何破坏性改动。
- 更容易复用代码：它通过自定义hooks来复用状态，从而解决了类组件逻辑难以复用的问题
- 函数式编程风格：函数式组件、状态保存在运行环境、每个功能都包裹在函数中，整体风格更清爽、优雅
- 代码量少，复用性高
- 更容易拆分

`Hooks缺点(Hoosk有哪些坑):`

- hooks 是 React 16.8 的新增特性、以前版本的就别想了
- 状态不同步（闭包带来的坑）:函数的运行是独立的，每个函数都有一份独立的闭包作用域。当我们处理复杂逻辑的时候，经常会碰到“引用不是最新”的问题
- 使用useState时候，使用push，pop，splice等直接更改数组对象的坑，demo中使用push直接更改数组无法获取到新值，应该采用析构方式`原因：push，pop，splice是直接修改原数组，react会认为state并没有发生变化，无法更新)`

### react-redux
为什么需要react-redux: 一个组件如果想从store存取公用状态，需要进行四步操作：import引入store、getState获取状态、dispatch修改状态、subscribe订阅更新，代码相对冗余,我们想要合并一些重复的操作，而react-redux就提供了一种合并操作的方案

react-redux提供Provider和connect两个API，Provider将store放进this.context里，省去了import这一步，connect将getState、dispatch合并进了this.props，并自动订阅更新

Provider: 使用React Contex 将store放入 this.context中
connect: 使用装饰器模式实现，所谓装饰器模式，简单地说就是对类的一个包装，动态地拓展类的功能。connect以及React中的高阶组件（HoC）都是这一模式的实现。


# Webpack

## 1.你用过哪些Loader

- `raw-loader`：加载文件原始内容（utf-8）
- `file-loader`：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
- `url-loader`：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值时返回其 publicPath，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
- `source-map-loader`：加载额外的 Source Map 文件，以方便断点调试
- `svg-inline-loader`：将压缩后的 SVG 内容注入代码中
- `image-loader`：加载并且压缩图片文件
- `json-loader` 加载 JSON 文件（默认包含）
- `handlebars-loader`: 将 Handlebars 模版编译成函数并返回
- `babel-loader`：把 ES6 转换成 ES5
- `ts-loader`: 将 TypeScript 转换成 JavaScript
- `awesome-typescript-loader`：将 TypeScript 转换成 JavaScript，性能优于 ts-loader
- `sass-loader`：将 CSS 代码注入 JavaScript 中，通过 DOM 操作去加载 CSS
- `css-loader`：加载 CSS，支持模块化、压缩、文件导入等特性
- `style-loader`：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS
- `postcss-loader`：扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀
- `eslint-loader`：通过 ESLint 检查 JavaScript 代码
- `tslint-loader`：通过 TSLint检查 TypeScript 代码
- `mocha-loader`：加载 Mocha 测试用例的代码
- `coverjs-loader`：计算测试的覆盖率
- `vue-loader`：加载 Vue.js 单文件组件
- `i18n-loader`: 国际化
- `cache-loader`: 可以在一些性能开销较大的 Loader 之前添加，目的是将结果缓存到磁盘里

## 2.你用过哪些Plugin

- `define-plugin`：定义环境变量 (Webpack4 之后指定 mode 会自动配置)
- `ignore-plugin`：忽略部分文件
- `html-webpack-plugin`：简化 HTML 文件创建 (依赖于 html-loader)
- `web-webpack-plugin`：可方便地为单页应用输出 HTML，比 html-webpack-plugin 好用
- `uglifyjs-webpack-plugin`：不支持 ES6 压缩 (Webpack4 以前)
- `terser-webpack-plugin`: 支持压缩 ES6 (Webpack4)
- `webpack-parallel-uglify-plugin`: 多进程执行代码压缩，提升构建速度
- `mini-css-extract-plugin`: 分离样式文件，CSS 提取为独立文件，支持按需加载 (替代extract-text-webpack-plugin)
- `serviceworker-webpack-plugin`：为网页应用增加离线缓存功能
- `clean-webpack-plugin`: 目录清理
- `ModuleConcatenationPlugin`: 开启 Scope Hoisting
- `speed-measure-webpack-plugin`: 可以看到每个 Loader 和 Plugin 执行耗时 (整个打包耗时、每个 Plugin 和 Loader 耗时)
- `webpack-bundle-analyzer`: 可视化 Webpack 输出文件的体积 (业务组件、依赖第三方模块)


## 3.Webpack构建流程简单说一下

Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

- `初始化参数`：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数
- `开始编译`：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译
- `确定入口`：根据配置中的 entry 找出所有的入口文件
- `编译模块`：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理
- `完成模块编译`：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系
- `输出资源`：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
- `输出完成`：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统

在以上过程中，`Webpack` 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。

简单说

- 初始化：启动构建，读取与合并配置参数，加载 Plugin，实例化 Compiler
- 编译：从 Entry 出发，针对每个 Module 串行调用对应的 Loader 去翻译文件的内容，再找到该 Module 依赖的 Module，递归地进行编译处理
- 输出：将编译后的 Module 组合成 Chunk，将 Chunk 转换成文件，输出到文件系统中

## 4. 简单说一下babel的原理

babel的转译过程分为三个阶段：**parsing、transforming、generating**，以ES6代码转译为ES5代码为例，babel转译的具体过程如下：

1\. ES6代码输入

2\. babylon 进行解析得到 AST

3\. plugin 用 babel-traverse 对 AST 树进行遍历转译,得到新的AST树

4\. 用 babel-generator 通过 AST 树生成 ES5 代码


# Network

### 在浏览器输入 URL 后

(1).浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址

(2).解析出 IP 地址后，根据该 IP 地址和默认端口 80 ，和服务器建立 TCP 连接

(3).浏览器发出读取文件（URL 中域名后面部分对应的文件）的 HTTP 请求，该请求报文作为 TCP 三次挥手的第三个报文的数据发送给服务器

(4).服务器对浏览器请求作出响应，并把对应的 html 文本发送给浏览器

(5).释放 TCP 连接

(6).浏览器将该 html 文本并显示内容

(7).浏览器将获取的 HTML 文档解析成 DOM 树

(8).处理 CSS 标记，构成层叠样式表模型 CSSOM

(9).将 DOM 和 CSSOM 合并成渲染树（rendering tree），将会被创建，代表一系列渲染对象

### 浏览器缓存

(1).强缓存：所请求的数据在缓存数据库中尚未过期时，不与服务器交互，直接使用缓存数据库中的数据，header 头 expires资源失效时间；cache-control 资源有效期或者缓存方式

(2).协商缓存：当强缓存过期未命中或者响应报文 Cache-Control 中有 must-redalidate 标识必须每次请求验证资源状态时，便使用协商缓存的方式去处理缓存文件。从缓存数据库中取出缓存的标识，然后向浏览器发送请求验证请求的数据是否已经更新，如果已更新则返回新的数据。若未更新则使用缓存数据库中的缓存数据。

注：强缓存不过服务器，协商缓存需要过服务器，可同时存在，强缓存优先级高，当执行强缓存，若命中缓存则直接使用缓存数据库中的数据，不再进行缓存协商。

### 跨域

(1).修改响应头：Access-Control-Allow-Origin 后端

(2).JSONP：只有 Ajax 请求存在跨域

# Websocket

Here is what I ended up with. It works for my purposes.

```javascript
function connect() {
  var ws = new WebSocket('ws://localhost:8080');
  ws.onopen = function() {
    // subscribe to some channels
    ws.send(JSON.stringify({
        //.... some message the I must send when I connect ....
    }));
  };

  ws.onmessage = function(e) {
    console.log('Message:', e.data);
  };

  ws.onclose = function(e) {
    console.log('Socket is closed. Reconnect will be attempted in 1 second.', e.reason);
    setTimeout(function() {
      connect();
    }, 1000);
  };

  ws.onerror = function(err) {
    console.error('Socket encountered error: ', err.message, 'Closing socket');
    ws.close();
  };
}

connect();
```

# Node.js

### 常用库及功能

http 模块：主要用于搭建http服务，处理用户请求信息，创建静态服务器

URL 模块：主要用于处理 url 的地址，方便对统一资源定位符进行操作处理

path 模块：处理磁盘路径，绝对路径

fs 模块：用于文件系统的增删改查

### 全局对象

- 1. node有哪些全局对象?

参考答案: process, console, Buffer

- 2. process有哪些常用方法?

参考答案: process.stdin, process.stdout, process.stderr, process.on, process.env, process.argv, process.arch, process.platform, process.exit

- 3. console有哪些常用方法?

参考答案: console.log/console.info, console.error/console.warning, console.time/console.timeEnd, console.trace, console.table

- 4. node有哪些定时功能?

参考答案: setTimeout/clearTimeout, setInterval/clearInterval, setImmediate/clearImmediate, process.nextTick

- 5. node中的事件循环是什么样子的?  
        总体上执行顺序是：process.nextTick >> setImmidate >> setTimeout/SetInterval 看官网吧： [https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)
- 6. node中的Buffer如何应用?

参考答案: Buffer是用来处理二进制数据的，比如图片，mp3,数据库文件等.Buffer支持各种编码解码，二进制字符串互转．


### child-process

- 1. 为什么需要child-process?

参考答案: node是异步非阻塞的，这对高并发非常有效．可是我们还有其它一些常用需求，比如和操作系统shell命令交互，调用可执行文件，创建子进程进行阻塞式访问或高CPU计算等，child-process就是为满足这些需求而生的．child-process顾名思义，就是把node阻塞的工作交给子进程去做．

- 2. exec,execFile,spawn和fork都是做什么用的?

参考答案: exec可以用操作系统原生的方式执行各种命令，如管道 cat ab.txt | grep hello; execFile是执行一个文件; spawn是流式和操作系统进行交互; fork是两个node程序(javascript)之间时行交互.

- 3. 实现一个简单的命令行交互程序?

参考答案: 那就用spawn吧.

代码演示

	var cp \= require('child\_process');

	var child \= cp.spawn('echo', \['你好', "钩子"\]); // 执行命令
	child.stdout.pipe(process.stdout); // child.stdout是输入流，process.stdout是输出流
	// 这句的意思是将子进程的输出作为当前程序的输入流，然后重定向到当前程序的标准输出，即控制台

- 4. 两个node程序之间怎样交互?

参考答案: 用fork嘛，上面讲过了．原理是子程序用process.on, process.send，父程序里用child.on,child.send进行交互.  
代码演示

	1) fork\-parent.js
	var cp \= require('child\_process');
	var child \= cp.fork('./fork-child.js');
	child.on('message', function(msg){
		console.log('老爸从儿子接受到数据:', msg);
	});
	child.send('我是你爸爸，送关怀来了!');

	2) fork\-child.js
	process.on('message', function(msg){
		console.log("儿子从老爸接收到的数据:", msg);
		process.send("我不要关怀，我要银民币！");
	});

- 5. 怎样让一个js文件变得像linux命令一样可执行?

参考答案: 1) 在myCommand.js文件头部加入 #!/usr/bin/env node 2) chmod命令把js文件改为可执行即可 3) 进入文件目录，命令行输入myComand就是相当于node myComand.js了

- 6. child-process和process的stdin,stdout,stderror是一样的吗?

参考答案: 概念都是一样的，输入，输出，错误，都是流．区别是在父程序眼里，子程序的stdout是输入流，stdin是输出流．