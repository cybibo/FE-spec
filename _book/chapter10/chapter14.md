浏览web文章的时候 收集的大厂的一些面试题 当然是查漏补缺,学习一波了

原文问题链接: https://www.nowcoder.com/discuss/122092?type=2&order=0&pos=334&page=2

happypack() 打包原理?
Happypack——将 loader 由单进程转为多进程 Happypack 会充分释放 CPU 在多核并发方面的优势，帮我们把任务分解给多个子进程去并发执行，大大提升打包效率

react 15 16 有什么变化?
Fiber体系的核心机制是负责任务调度的ReactFiberScheduler，相当于之前的ReactReconciler
vDOM tree变成fiber tree了，以前是自上而下的简单树结构，现在是基于单链表的树结构，维护的节点关系更多一些

16的新特性:
error boundaries：错误边界处理
Error Boundary可以看作是一种特殊的React组件，新增了componentDidCatch这个生命周期函数，它可以捕获自身及子树上的错误并对错误做优雅处理，包括上报错误日志、展示出错提示，而不是卸载整个组件树。（注：它并不能捕获runtime所有的错误，比如组件回调事件里的错误，可以把它想象成传统的try-catch语句）

render方法新增返回类型
在React 16中，render方法支持直接返回string，number，boolean，null，portal，以及Fragments(带有key属性的数组)，这可以在一定程度上减少页面的DOM层级

portals ：支持 createPortal 声明性地将子树渲染到另一个DOM节点 默认情况下，React组件树和DOM树是完全对应的，因此对于一些Modal,Overlay之类的组件，通常是将它们放在顶层，但逻辑上它们可能只是属于某个子组件，不利于组件的代码组织。通过使用createPortal，我们可以将组件渲染到我们想要的任意DOM节点中，但该组件依然处在React的父组件之内。带来的一个特性就是，在子组件产生的event依然可以被React父组件捕获，但在DOM结构中，它却不是你的父组件。对于组件组织，代码切割来说，这是一个很好的属性

custom DOM attributes ：ReactDom允许传递非标准属性
在之前的版本中，React会忽略无法识别的HTML和SVG属性，自定义属性只能通过data-*形式添加，现在它会把这些属性直接传递给DOM，这个改动让React可以去掉属性白名单，从而减少了文件大小。但当DOM传递的自定义属性是函数类型或event handler类型时，也会被React忽略掉

improved server-side rendering:提升服务端渲染性能
提升服务端渲染性能，React 16的SSR被完全重写，新的实现非常快，接近3倍性能于React 15，现在提供一种流模式streaming，可以更快地把渲染的字节发送到客户端

setState 的改变:
调用setState返回null将不会更新render，这样可以让你在更新方法中自己决定是否更新
this.setState(() => {}) 建议用函数的写法
当两个组件 和 <B / >发生替换时，B.componentWillMount总是会在A.componentWillUnmount之前执行，而在之前，A.componentWillUnmount有可能会提前执行
之前版本，当改变一个组件的ref时，ref和dom会在组件的render方法被调用之前分离。现在，我们延迟了ref的改变，直到dom元素被改变了，ref才会和dom分离
现在setState回调（第二个参数）会在componentDidMount/componentDidUpdate后立即触发，而不是等到所有组件渲染完成之后

新的打包策略: 新的打包策略中去掉了process.env检查, React 16的体积比上个版本减小了32%（30% post-gzip），文件尺寸的减小一部分要归功于打包方法的改变

componentDidUpdate生命周期不再接受prevContext参数

使用不唯一的key可能会导致子组件的复制或者遗失，使用不唯一的key并不支持，并且也从未支持，但之前这是一个硬性错误

Shallow renderer（浅层渲染）不再触发componentDidUpdate()，因为DOM的refs是不可用的。这也使得它与componentDidMount()之前版本中的调用一致

服务器渲染不再使用标记验证，而是尽力附加到现有的DOM，警告不一致。它也不再使用每个节点上的空白组件和数据反馈属性的注释, 为服务器渲染容器现在有一个明确的API。使用ReactDOM.hydrate而不是ReactDOM.render如果你正在恢复服务器呈现的HTML。继续使用，ReactDOM.render如果你只是做客户端渲染

废弃了: componentWillMount componentWillRecieveProps componentWillUpdate
新增: getDerivedStateFromProps getSnapshotBeforeUpdate

不再构建react-with-addons.js，所有兼容的插件都是在npm上单独发布的，如果你需要它们，可以使用单文件浏览器版本

Fiber架构?
改变了之前react的组件渲染机制，新的架构使原来同步渲染的组件现在可以异步化，可中途中断渲染，执行更高优先级的任务。释放浏览器主线程(包括用户的点击，鼠标移动等操作)
因为JavaScript在浏览器的主线程上运行，恰好与样式计算、布局以及许多情况下的绘制一起运行。如果JavaScript运行时间过长，就会阻塞这些其他工作，可能导致掉帧。React希望通过Fiber重构来改变这种不可控的现状，进一步提升交互体验
增量渲染（把渲染任务拆分成块，匀到多帧）
更新时能够暂停，终止，复用渲染任务
给不同类型的更新赋予优先级
并发方面新的基础能力

webpack 打包速度优化 exclude cacheDirectory dll happypack ?
UglifyJsPlugin 的代码其实是 webpack3 的用法，webpack4 现在已经默认使用 uglifyjs-webpack-plugin 对代码做压缩了——在 webpack4 中，我们是通过配置 optimization.minimize 与 optimization.minimizer 来自定义压缩相关的操作的
loader: ‘babel-loader?cacheDirectory=true’ 在一些性能开销较大的 loader 之前添加此 loader，以将结果缓存到磁盘里, 请注意，保存和读取这些缓存文件会有一些时间开销，所以请只对性能开销较大的 loader 使用此 loader
webpack 的缺点，好在我们的 CPU 是多核的，Happypack 会充分释放 CPU 在多核并发方面的优势，帮我们把任务分解给多个子进程去并发执行，大大提升打包效率,HappyPack 的使用方法也非常简单，只需要我们把对 loader 的配置转移到 HappyPack 中去就好，我们可以手动告诉 HappyPack 我们需要多少个并发的进程

配置 router 的 按需加载: require.ensure(dependencies, callback, chunkName)

1
2
3
4
5
6
7
const getComponent => (location, cb) {
  require.ensure([], (require) => {
    cb(null, require('../pages/BugComponent').default)
  }, 'bug')
},

<Route path="/bug" getComponent={getComponent}>
Cookie 是紧跟域名的。同一个域名下的所有请求，都会携带 Cookie
这样就凸显了 Local Storage 的优越性

redux 实质性的工作流?
React通过Context属性，可以将属性(props)直接给子孙component，无须通过props层层传递, Provider仅仅起到获得store，然后将其传递给子孙元素而已
connect是一个高阶函数，首先传入mapStateToProps、mapDispatchToProps，然后返回一个生产Component的函数(wrapWithConnect)，然后再将真正的Component作为参数传入wrapWithConnect(MyComponent)，这样就生产出一个经过包裹的Connect组件，该组件具有如下特点:
通过 props.store 获取祖先 Component 的 store
props包括stateProps、dispatchProps、parentProps,合并在一起得到nextState，作为props传给真正的Component
componentDidMount时，添加事件this.store.subscribe(this.handleChange)，实现页面交互
shouldComponentUpdate时判断是否有避免进行渲染，判断得到的 nextState,提升页面性能
componentWillUnmount时移除注册的事件this.handleChange
在非生产环境下，带有热重载功能
// 主要的代码逻辑
export default function connect(mapStateToProps, mapDispatchToProps, mergeProps, options = {}) {}

Reducer是一个纯函数（Pure Function） （相同的输入会得到同样的输出）

z-index 再fixed 中的表现?
子元素 z-index设置成负数 会被父元素覆盖
左侧的菜单固定为fixed时，二级菜单无法设置有效的z-index，导致菜单隐藏在页面元素之下，明明页面元素的z-index是1，但是无论把菜单的z-index设置为多大，都不管用?
原来谷歌浏览器在设置position:fixed;后会触发元素创建一个新的层叠上下文，并且当成一个整体在父层叠上下文中进行比较。如上面的dom结构，当给b设置了position:fixed;属性后，会触发创建一个新的层叠上下文，它的父层叠上下文变成了a，所以b只能在a的内部进行层叠比较。这也就是大家熟听的“从父原则”
所以本来所有元素都在root内比较，改为fixed之后只能在父级元素内比较，而父级元素没有设置z-index，所以无法比较
所以解决方案是给父级元素设置z-index，一般设置为0就可以了
摘抄两点:
1.z-index只有在设置了position为relative,absolute,fixed时才会有效。
2.z-index的“从父原则”。当你发现把z-index设的多大都没用时，看看其父元素是否设置了有效的z-index，当然别忘了先看看自身是否设置了position

Etag 的生成过程需要服务器额外付出开销，会影响服务端的性能，这是它的弊端。因此启用 Etag 需要我们审时度势。正如我们刚刚所提到的——Etag 并不能替代 Last-Modified，它只能作为 Last-Modified 的补充和强化存在。 Etag 在感知文件变化上比 Last-Modified 更加准确，优先级也更高。当 Etag 和 Last-Modified 同时存在时，以 Etag 为准

当我们的资源内容不可复用时，直接为 Cache-Control 设置 no-store，拒绝一切形式的缓存；否则考虑是否每次都需要向服务器进行缓存有效确认，如果需要，那么设 Cache-Control 的值为 no-cache；否则考虑该资源是否可以被代理服务器缓存，根据其结果决定是设置为 private 还是 public；然后考虑该资源的过期时间，设置对应的 max-age 和 s-maxage 值；最后，配置协商缓存需要用到的 Last-Modified Etag 等参数

CDN 的核心点有两个，一个是缓存，一个是回源

1.JavaScript高级程序设计（红宝书），看个两三遍..其义自见，面试内容基本逃不过红宝书里的东西。继承、原型链、作用域链百考不厌。

2.es6标准入门（阮一峰），不要只是了解es6有哪些东西，建议直接看线上版，一个一个块去学习..面试官问es6了解哪些的时候你说的越多评价越高，比较核心的内容：箭头函数，promise,map,set,let,const,class,symbol,generator。es7:async,await

3.玩转数据结构（慕课网，网上可以找到百度云资源），非常重要，前端同学不要觉得数据结构没用.. 实际上了解更多的数据结构可以让你编码更加轻松和流畅（解析后台数据的时候也会更加清楚怎么做）。还有就是一定要跟着写，像链表、队列、二叉树、堆跟着写一下就好..面试过程中有遇到手写bst的add..前中后序遍历..删除节点

4.剑指offer和LeetCode，不管你觉得前端需不需要会算法，刷就行了

5.个人技术栈是vue。针对vue：双向数据绑定原理（被问到吐，最好会写一个简单的双绑），v-model原理（快手挂掉的原因），diff算法（考得较少），vue和其他框架的区别（一般react），vue代码优化，组件编写要点，vue-router原理（如何加入动态参数），vuex解决了什么有哪些模块

6.移动端（个人有半年左右的移动端开发经历，所以问得较多）：300ms产生原因和解决方案，点击穿透事件，如何做自适应，兼容性问题如何解决，input框被输入法遮挡解决方案

7.计算机网络，非常非常重要，大厂必问。osi七层模型/tcpip四层模型，http1.0 1.1 2.0区别，https原理，请求响应报文header具体内容（了解的越多越好），请求方式，各种响应码（最重要的304一定要说清楚，详见http缓存详解，cache-control），tcp/udp不同，tcp（三握四挥、syn洪泛、流量控制、拥塞控制、滑动窗口协议），dns解析，个人还被问过mac/ip。

8.os：进线程区别，调度和通信方式

9.数据库：除了事务以外我不会..一般不会问

10.前端优化问题（各种方案，最好能手写）

11.比较常考的前端代码题：节流防抖、bind底层、extend底层、$底层、cookie封装、扁平化、柯里化、promise原理、手写闭包、手写ajax、串行ajax请求处理、url处理、promise封装ajax。

12.设计模式（单例、工厂、观察者、订阅发布者），最好能手写

13.浏览器机制（异步机制、线程宿主环境），微宏任务（非常重要），URL输入到绘制的全过程

14.后台（基本没被问过..），楼主也只会点点nodejs，入门级别（会写爬虫，简单搭建服务器，了解koa洋葱圈模型，express中间件写法）

15.html:h5相关内容，加分点：canvas 和 webgl

16.css：选择器相关、双列等高三列自适应布局等、垂直居中（可能不定高）、动画（多写..js动画了解jq和velocity）、css3相关（transition/transform）、bfc（形成方式）、display/position参数、flex参数，border相关绘图，box-sizing参数

17.spa相关（首屏加载，白屏问题，路由转换，seo）

18.服务器端渲染（不会问太深，加分项）

19.websocket原理和服务器端推送机制，长短轮询
WebSocket：WebSocket一种在单个 TCP 连接上进行全双工通讯的协议
客户端发送一次http websocket请求，服务器响应请求，双方建立持久连接，并进行双向数据传输，后面不进行HTTP连接，而是使用TCP连接
Websocket使用和 HTTP 相同的 TCP 端口，可以绕过大多数防火墙的限制。默认情况下，Websocket协议使用80端口；运行在TLS之上时，默认使用443端口
Upgrade: websocket
Connection: Upgrade

在HTTP/1.0中默认使用短连接。也就是说，客户端和服务器每进行一次HTTP操作，就建立一次连接，任务结束就中断连接
而从HTTP/1.1起，默认使用长连接，用以保持连接特性。使用长连接的HTTP协议，会在响应头加入 Connection:keep-alive

响应式的rem实现原理:
rem是相对于根元素（html标签）的字体大小的单位
核心思想：百分比布局可实现响应式布局，而rem相当于百分比布局
实现手段：动态获取当前视口宽度width，除以一个固定的数n，得到rem的值。表达式为rem = width / n
通过此方法，rem大小始终为width的n等分

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
// 核心代码块：
// 动态为根元素设置字体大小
function init () {
　// 获取屏幕宽度
  var width = document.documentElement.clientWidth || window.innerWidth
　// 设置根元素字体大小。此时为宽的10等分
  document.documentElement.style.fontSize = width / 10 + 'px'
}
// 首次加载应用，设置一次
init()
// 监听手机旋转的事件的时机，重新设置
window.addEventListener('orientationchange', init)
// 监听手机窗口变化，重新设置
if (resizeTimeout) {
  clearTimeout(resizeTimeout)
  resizeTimeout = null
}
var resizeTimeout = setTimeout(() => {
  window.addEventListener('resize', init)
}, 30)
// 理解：上面代码实现了，无论设备可视窗口如何变化，始终设置rem为width的1/10.即实现了百分比布局
tips：
　　1、以上代码需在dom之前写入（可放在head里面第一个script标签）
　　2、移动端加上meta标签
也可以用 flexible.js 动态修改rem的值

render ?
Virtual DOM => Actual DOM
Virtual DOM 通过 vm.$createElement(‘div’)生成一个JS对象 { tag: ‘div’, data: { attrs: {}, …}, children: [] }
Actual DOM 通过document.createElement(‘div’)生成一个DOM节点

精确获取页面元素位置的方式有哪些?
那就是使用getBoundingClientRect()方法 它返回一个对象，其中包含了left、right、top、bottom四个属性

1
2
3
4
5
var X= this.getBoundingClientRect().left
var Y =this.getBoundingClientRect().top
//再加上滚动距离，就可以得到绝对位置
var X= this.getBoundingClientRect().left+document.documentElement.scrollLeft
var Y =this.getBoundingClientRect().top+document.documentElement.scrollTop
ES6 的 super 关键字表示父类的构造函数, 相当于 ES5 的 Parent.call(this) 所以子类只有调用 super 之后, 才能使用 this 关键字
object.setPrototypeOf(child.prototype, parent.prototype)

a b c 按顺序执行?
Promise.resolve() 构建队列:

1
2
3
4
5
6
7
8
9
10
11
function queue (arr) {
  let sequence = Promise.resolve()
  arr.forEach (function (item) {
    sequence = sequence.then(item)
  })
  return sequence
}

queue([a, b, c]).then( data => {
  console.log(data)  // abc
})
async await 构建队列:

1
2
3
4
5
6
7
8
9
10
11
async function queue(arr) {
  let res = null
  for (let promise of arr) {
    res = await promise(res)
  }
  return await res
}

queue([a, b, c]).then( data => {
  console.log(data)  // abc
})
url中的# ?
window.location.hash 来读取 或 改变 #
location.href += ‘#caper’ 浏览器滚动到新的页面位置

H5的 onhashchange 事件 的3中使用方式:

1
2
3
window.onhashchange = func
<body onhashchange = 'func()' ></body>
window.addEventListener('hashchange', func, false)
