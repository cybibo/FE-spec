浏览web文章的时候 收集的大厂的一些面试题 当然是查漏补缺,学习一波了

原文问题链接: https://juejin.im/post/5bc92e9ce51d450e8e777136

—————- 阿里-网易-滴滴 —————-

————————— 阿里 —————————
使用过的koa2中间件
koa-body原理
介绍自己写过的中间件
有没有涉及到Cluster
介绍pm2
master挂了的话pm2怎么处理
如何和MySQL进行通信
——- node的东西后面再解答 ————
React声明周期及自己的理解?
react的生命周期有:
(getDefaultProps getInitialState)
componentWillMount render componentDidMount 完成初始化赋值, 渲染DOM, 初始数据请求并再次渲染 只执行一次
componentWillReciveProps(nextProps) 提前根据传入的数据 修改当前组件的state 并不会重复出发render
shouldComponentUpdate(nextProps, nextState) componentWillUpdate render componentDidUpdate 决定是否更新 提升性能
componentWillUnmount 卸载 清空定时任务 网络请求等

如何配置React-Router?
1.标签的方式: BrowserRouter/HashRouter Router Switch exact Route path component this.props.children redirect 通过 path 跟 Component 对应的方式
2.对象配置的方式: {path component indexRoute childRoutes onLeave onEnter }
3.按需加载的方式配置路由 提升性能 Route 可以定义 getIndexRoute getComponents getChildRoutes 几个异步函数 只有需要的时候才调用

路由的动态加载模块?
根据路由组件打包成多个 bundle，只有在点击到对应的 Route 时，这个 bundle 才会被加载
webpack babel-plugin-systax-dynamic-import react-loadable

1
2
3
4
5
6
<Route exact path="/settings"
  component={Loadable({
    loader: () => import(/* webpackChunkName: "Settings" */ './Settings.js'),
    loading:Loading
  })}
/>
服务端渲染SSR ?
主要是利于SEO 减少首页白屏
理解: 通过后台(新的 renderToNodeStream 替代 原始的 renderToString)最后返回已经把数据处理好的 html 页面(link => css, div#root, script)
css-modules-require-hook/preset (csshook) 处理 css 引入
express body-parser cookie-parser 处理 js
react-router-dom 处理后端路由
react react-dom 解析jsx语法
redux react-redux react-thunk 处理 全局数据 (createStore applyMiddleware compose)

介绍路由的history?
它属于 Bom 浏览器对象, 常用来原生实现路由跳转但是不刷新页面的功能 记录浏览历史记录 用来实现前进后退的功能,
中间可以做一些数据处理,与信息提示
H5新增了两个API: history.pushState 和 history.replaceState
接收3个参数 (状态对象object, 标题title, 地址url)

介绍Redux数据流的流程?
view => action/dispatch => store(reducer) => view

Redux如何实现多个组件之间的通信，多个组件使用相同状态如何进行管理 ?
react-redux(Provider 传入到最外层组件store 在需要用到的地方 用 connect 获取 (mapStateToProps, mapDispatchToProps) 在页面中引用)
类似发布订阅模式, 一个地方修改了这个值, 其他所有使用了这个相同状态的地方也会更改

多个组件之间如何拆分各自的state，每块小的组件有自己的状态，它们之间还有一些公共的状态需要维护，如何思考这块?
一个全局的 reducer , 页面级别的 reducer , 然后redux 里有个 combineReducers 把所有的 reducer 合在一起,小组件的使用与全局的使用是分开的互不影响

使用过的Redux中间件?
redux react-redux redux-thunk(把action 返回的对象 换成一个异步函数) redux-saga

如何解决跨域的问题?
iframe nginx node jsonp cors(在header中 设置 Access-Control-Allow-Origin)
H5的 window.postMessage

常见Http请求头?
:authority: www.cnblogs.com
:method: GET
:path: /mvc/blog/ViewCountCommentCout.aspx?postId=6958181
:scheme: https
accept: application/json, text/javascript, /; q=0.01
accept-encoding: gzip, deflate, br
accept-language: zh-CN,zh;q=0.9,en;q=0.8
content-type: text
cookie: ‘’,
token: ‘’,
referer: https://www.cnblogs.com/,
user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36
x-requested-with: XMLHttpRequest

移动端适配1px的问题?
代码生成 0.5px 像素边框 页面展示的时候就是 1px 大小了
缩放 transform: scaleY(0.5)
虚线 background: linear-gradient(0 deg, #fff, #000)
边框阴影 box-shadow: 0 0.5px 0 #000;
图片 svg图片 base64 的细线

介绍flex布局?
display: flex , direction row colum 设置主轴和垂直的轴
just-comtent aligin-items 来设置两个轴的元素节点

其他css方式设置垂直居中?
父元素 line-height 与 height 同高
transform: translate(-50%, -50%)
just-comtent:center aligin-items:center

居中为什么要使用transform（为什么不使用marginLeft/Top?
相对于自生元素的大小 更加可控

使用过webpack里面哪些 plugin 和 loader?
plugin:
DllReferencePlugin => 生成 manifest.json 文件 利于缓存页面数据
html-webpack-plugin => 把带hash的dll js/css插入到html中
copy-webpack-plugin => 把 src 目录外的文件(static) 引入到 dist 目录里
happypack => 因为js是单线程的 多进程来分别执行这些线程 完成的时候在 event loop 中返回给 webpack 处理 提高打包效率
webpack-bundle-analyzer => 用来分析打包的大小 针对性的去处理文件
DefinePlugin => 注入一些全局的 config 区分开发和生产
mini-css-extract-plugin => 压缩 css 代码

loader:
url-loader => 小图片转化成 base64 进行加载
styly-loader css-loader postcss-loader less-loader => 处理 css
eslint-loader => 代码检查
vue-loader react-loader => 处理框架的语法

webpack里面的插件是怎么实现的?
插件开发，最重要的两个对象：compiler、compilation
compiler.plugin(‘***’)就相当于给compiler设置了事件监听
所以compiler.plugin(‘compile’)就代表：当编译器监听到compile事件时，我们应该做些什么
compilation（’编译器’对’编译ing’这个事件的监听）相当于对编译过程的监听
compiler.plugin(“emit”, function(compilation, callback) {} 最后执行 emit 输出的回调函数 callback

dev-server是怎么跑起来?
这个用 node 搭建本地资源服务器类似的
devServer: {
contentBase: ‘./‘, 服务器搭建在当前目录
historyApiFallback:true,
inline：true,
hot：true,
port: 8080
}

项目优化?
项目的优化是一根线上的优化, 即从一个网址请求到页面渲染的各个相关的环节都有优化, 这个还包括项目代码的优化
DNS 解析
TCP 连接
HTTP 请求抛出
服务端处理请求，HTTP 响应返回
浏览器拿到响应数据，解析响应内容，把解析的结果展示给用户
(构建功能及性能优化, 图片优化, 浏览器缓存, 离线存储技术, 服务端渲染, css性能方案, js性能方案, 重绘回流, 事件循环异步更新,懒加载, 事件节流与防抖)

抽取公共文件是怎么配置的?
用 webpack 打包 处理 dll

1
2
3
new webpack.DllReferencePlugin({
  manifest: path.resolve(ROOT_PATH, `dist/dll/${NODE_ENV}/vendor-manifest.json`)
})
项目中如何处理安全问题?
HTTPS请求
登陆的时候加图片验证 ,一般的登陆密码等 加 md5 加盐处理
重要数据的防爬虫的页面数据组合展示
所有的请求在 http header 里 带上 token

怎么实现this对象的深拷贝?
var obj1 = { body: { a: 10 } }
var obj2 = JSON.parse(JSON.stringify(obj1))

lodash.cloneDeep()

Object.create()

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
var obj = finalObj || {}
  for (var i in initalObj) {
    var prop = initalObj[i];        // 避免相互引用对象导致死循环，如initalObj.a = initalObj的情况
    if(prop === obj) {
      continue;
    }
    if (typeof prop === 'object') {
      obj[i] = (prop.constructor === Array) ? [] : Object.create(prop);
    } else {
      obj[i] = prop;
    }
  }
  return obj;
}
vue 的双向绑定的原理是什么 ?
vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。
　　具体步骤：
　　第一步：需要 observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter
这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化
　　第二步：compile解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图
　　第三步：Watcher订阅者是Observer和Compile之间通信的桥梁，主要做的事情是:
　　1、在自身实例化时往属性订阅器(dep)里面添加自己
　　2、自身必须有一个update()方法
　　3、待属性变动dep.notice()通知时，能调用自身的 update() 方法，并触发Compile中绑定的回调，则功成身退。
　　第四步：MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果

————————— 网易 —————————
介绍redux，主要解决什么问题?
不好用 props 去传递使用的一些公共的数据的管理

文件上传如何做断点续传?
用 H5 的 File api
先得获得一个文件ID 文件HASH值的思路就是：MD5（文件名称+文件长度+文件修改时间+自定义的浏览器ID）
在上传文件之前，从服务端查询文件的断点续传信息, 决定从什么位置上传数据
关键技术点是：从上次上传的长度位置上传
var blob = fileObj.slice(start_offset,filesize)
function upload_file(fileObj, start_offset, fileid){}

表单可以跨域吗?
本身是不能跨域的, 提交 action 是一步性操作, 可以模拟 post 方法进行跨域请求

promise、async有什么区别?
异步编程的最高境界，就是根本不用关心它是不是异步
async 能够 使异步事件用同步的方法写, 不用写太多嵌套代码,代码可读性更高
错误抓取 Promise.catch(){} try {} catch {}
promise的箭头函数中不能打断点, async 只关心返回的结果

搜索请求如何处理（防抖）?
防抖 : 是限制下次函数调用之前必须等待的时间间隔(300ms)。将若干个函数调用合成 一次，并在给定时间过去之后仅被调用一次
节流: 节流函数允许一个函数在规定的时间内只执行一次

搜索请求中文如何请求?
把中文 替换成字符编码 再发请求

介绍观察者模式?
把自己注入到被观察的对象里, 对象改变某个数据的时候, 调用自己的update函数更新相应的状态, 强耦合的方式

观察者和订阅-发布的区别，各自用在哪里?
两者的主要区别是调度的地方不同 订阅发布模式比观察者模式，中间多一个“调度中心”。因此更解耦，所以常见系统中，订阅发布模式能让业务更清晰
观察者模式是由具体目标调度的 而发布/订阅模式是统一由调度中心调的，所以观察者模式的订阅者与发布者之间是存在依赖的，而发布/订阅模式则不会
可以把restful请求的通信方式，看做观察者模式的应用；而服务总线（MQ）的方式，则是订阅发布模式

介绍中介者模式?
中介对象主要是用来封装行为的，行为的参与者就是那些对象，但是通过中介者，这些对象不用相互知道(机场交通控制系统)
中介者模式与业务相关，订阅/发布模式与业务无关
虽然实现结构非常相像，但是很明显的是，中介者模式参与业务相关东西，所以内容是大相径庭的

介绍react优化?
this的绑定方法 在constructor中绑定事件
在父组件因状态的变化更改，而子组件并没有状态变化时, 不让子组件render shouldComponentUpdate(nextProps, nextState) React.PureComponent 替换 React.Component
加key
redux性能优化 使用reselect库 在调用到已经执行过的数据时，react不会再次对数据进行渲染，而是从reselector中取出缓存数据加载，减少了重新渲染，达到性能优化的效果
用函数子组件进行UI和逻辑的分离, 拆分成更小组件, 数据的变更改变尽量少的组件
一些与状态无关的属性不要放在state里, 避免数据改变重新渲染dom

介绍http2.0?
HTTP2.0相比HTTP1.1可以给用户带来更佳的用户体验
新增 二进制分帧传输 改进传输性能，实现低延迟和高吞吐量
压缩头部 用 首部表 来跟踪和存储之间发送的 键-值 对
多路复用 并行请求
服务端推送（Server Push）

通过什么做到并发请求?
改接口让后台一次请求查询更多的数据,减少请求
并行改为串行
jquery 的 var d2 = $.Deferred()
使用 promise.all 统一处理所有的请求数据

http1.1时如何复用tcp连接?
tcp 长连接 本来就是复用的

介绍service worker
服务端推送消息 不用轮询

redux请求中间件如何处理并发
还是那一套吧, 防抖 节流 合并请求 promise.all等

介绍Promise，异常捕获
Promise.catch() promise.then().catch() try{}catch{}

介绍position属性包括CSS3新增
static fixed absolute relative inherit
position:sticky
该元素并不脱离文档流，仍然保留元素原本在文档流中的位置
当元素在容器中被滚动超过指定的偏移值时，元素在容器内就固定在指定位置
元素固定的相对偏移是相对于离它最近的具有滚动框的祖先元素，如果祖先元素都不可以滚动，那么是相对于viewport来计算元素的偏移量

浏览器事件流向?
事件捕获 处于目标 事件冒泡

介绍事件代理以及优缺点?
事件代理发生在事件冒泡的处理上 , 可以不用每个子元素都绑定事件处理的方法, 动态的子元素也可以触发冒泡
如果把所有事件都用事件代理，可能会出现事件误判,

React组件中怎么做事件代理,及其原理?
区别于浏览器事件处理方式，React并未将事件处理函数与对应的DOM节点直接关联，而是在顶层使用了一个全局事件监听器监听所有的事件
React会在内部维护一个映射表记录事件与组件事件处理函数的对应关系
当某个事件触发时，React根据这个内部映射表将事件分派给指定的事件处理函数
当映射表中没有事件处理函数时，React不做任何操作
当一个组件安装或者卸载时，相应的事件处理函数会自动被添加到事件监听器的内部映射表中或从表中删除

介绍this各种情况?
指向当前function object对象 react组件实例 window

前端怎么控制管理路由?
标签的方式 对象配置的方式 动态路由

使用路由时出现问题如何解决?
Switch匹配 最后一个路由放404的页面
全局的路由拦截, 例如登陆token的校验

React怎么做数据的检查和变化?
prop-types 第三方插件检查数据的类型 以及 isRequired 必须传入
componentWillRecieveProps(nextProps) 对比传入数据的变化

————————— 滴滴 —————————
react-router怎么实现路由切换?


定义路由的时候path 和 component 一一对应, 页面的路由改变 就加载对应的组件

a标签 this.props.history.push(‘path’) withRouter(Component) window.$history.push(‘path’)

react-router里的标签和标签有什么区别?
Link只加载对应路由的组件,页面不会重新渲染 a标签实现页面跳转

标签默认事件禁掉之后做了什么才实现了跳转?
href=’javascript:void(0);’ 在 onClick={} 处理页面跳转

React层面的性能优化?

整个前端性能提升大致分几类?

import { Button } from ‘antd’，打包的时候只打包button，分模块加载，是怎么做到的?
webpack 打包前会把所有的组件 分成一个个的小 chunk
当页面加载的时候再用 getComponents方法 引入到项目里, 实现分模块加载

使用import时，webpack对node_modules里的依赖会做什么?
一般直接引入组件 不做压缩编译等 设置loader 的时候 exclude: /node_modules/

JS异步解决方案的发展历程以及优缺点?
function(callback){} promise.then().catch() promise.all([]) generater/yield async/await

Http报文的请求会有几个部分?
请求行 请求头 数据体
状态行 HTTP/1.1 200 ok，响应头，响应正文

cookie放哪里，cookie能做的事情和存在的价值?
后端生成 放在 http的请求头里 一般5k大小 用于身份认证的功能 携带少量的信息 每次请求都会带上

cookie和token都存放在header里面，为什么只劫持前者?
cookie 每次请求都会带上 内容主要包括：名字，值，过期时间，路径和域,cookie是有状态的 被劫持不安全, 可以设置httpOnly 防止 cors
token可以设置失效时间, 是无状态的 被劫持后危险性要低一些, 在跨端能力上更好

cookie和session有哪些方面的区别?
cookie 机制采用的是在客户端保持状态的方案 session 机制采用的是在服务器端保持状态的方案
cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗 考虑到安全应当使用session
session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能 考虑到减轻服务器性能方面，应当使用COOKIE
单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie
将登陆信息等重要信息存放为SESSION 其他信息如果需要保留，可以放在COOKIE中

React中Dom结构发生变化后内部经历了哪些变化?
diff算法对比虚拟dom与现在视图dom, 计算哪些节点需要重新渲染 算出最小操作集 然后到组件 render

React挂载的时候有3个组件，textComponent、composeComponent、domComponent，区别和关系，
Dom结构发生变化时怎么区分data的变化，怎么更新，更新怎么调度，如果更新的时候还有其他任务存在怎么处理?
ReactClass
他们都接收node参数
如果node是null，则生成ReactDOMEmptyComponent
如果node是数字或字符串，则生成eactDOMTextComponent
如果传入的node是一个对象，是ReactElement对象,分别生成ReactDOMComponent和ReactCompositeComponent
虽然是不同的对象，但是都实现了mountComponent,receiveComponent和unmountComponent三个关键的方法
mountComponent方法用于把ReactElement转化为HTML标记，最终挂载到DOM上，而经浏览器解析后的DOM元素，就是用户视角看到的React组件了
更新的时候,继续diff 算法 同上

key主要是解决哪一类的问题，为什么不建议用索引index（重绘）?
循环渲染子元素 加个key值使diff算法更快, 使用index的话, 当数组增删的时候, dom元素对应index改变 会出现渲染的问题

Redux中异步的请求怎么处理?
react-thunk react-saga 把返回action对象 改成异步函数处理并返回

Redux中间件是什么东西，接受几个参数（两端的柯里化函数）?
中间件是怎么拿到store和action，然后怎么处理?
处理异步函数 打印logger信息等

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
const logger = store => next => action => {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}

// 当我们调用applyMiddleware方法时
applyMiddleware(store,[logger])

// 会依次middlewares数组中的方法，并且每次执行都重新封装store.dispatch
middlewares.forEach(middleware =>
  dispatch = middleware(store)(dispatch)
)

// 这里体现了函数柯里化，每次执行一个中间件，middleware(store)(dispatch),第二个括号内的（dispatch），传递给了具体函数的next
柯里化函数两端的参数具体是什么东西?
middleware(store)(dispatch) 第二个括号内的（dispatch），传递给了具体函数的next

state是怎么注入到组件的，从reducer到组件经历了什么样的过程?
react-redux connect(state, dispatch)(component)
store => reducers => value => this.props

koa中response.send、response.rounded、response.json发生了什么事，浏览器为什么能识别到它是一个json结构或是html?

koa-bodyparser怎么来解析request?
var Koa = require('koa')
var bodyParser = require('koa-bodyparser')
var app = new Koa()
app.use(bodyParser())
app.use(async ctx => {
  ctx.body =  ctx.request.body
}
webpack整个生命周期，loader和plugin有什么区别?
对于loader，它就是一个转换器，将A文件进行编译形成B文件，这里操作的是文件，比如将A.scss或A.less转变为B.css，单纯的文件转换过程
对于plugin，它就是一个扩展器，它丰富了wepack本身，针对是loader结束后，webpack打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听webpack打包过程中的某些节点
run：开始编译
make：从entry开始递归分析依赖并对依赖进行build
build-moodule：使用loader加载文件并build模块
normal-module-loader：对loader加载的文件用acorn编译，生成抽象语法树AST
program：开始对AST进行遍历，当遇到require时触发call require事件
seal：所有依赖build完成，开始对chunk进行优化（抽取公共模块、加hash等）
optimize-chunk-assets：压缩代码
emit：把各个chunk输出到结果文件
通过对节点的监听，从而找到合适的节点对文件做适当的处理

介绍AST（Abstract Syntax Tree）抽象语法树?
虚拟dom 类似 render(‘div’, ‘data-‘, text)

安卓Activity之间数据是怎么传递的?
Activity_1利用Intent的putExtra()方法来携带数据，然后Activity_2通过Intent的getExtra()方法来获取Activity_1传递过来的数据
利用startActivityForResult()这个方法来启动Activity_2,然后Activity_2在利用Intent和setResult()方法来向Activity_1传送数据，最后，Activity_1通过回调方法onActivityResult()来接收Activity_2数据

WebView和原生是如何通信?
1.JSbridge：:(webviewJavascriptBridge)一种js与原生native通信的机制，可以h5与native互调
2.Cordova

跨域怎么解决，有没有使用过Apache等方案?
node代理 nginx cors
启用Apache 的 proxy module反向代理解决js跨域问题
配置完成之后，访问 http://localhost/project 实际就是访问 http://ip_address/project 上的资源 来实现跨域

vue 的双向绑定的原理是什么 ?
vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。
　　具体步骤：
　　第一步：需要 observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter
这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化
　　第二步：compile解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图
　　第三步：Watcher订阅者是Observer和Compile之间通信的桥梁，主要做的事情是:
　　1、在自身实例化时往属性订阅器(dep)里面添加自己
　　2、自身必须有一个update()方法
　　3、待属性变动dep.notice()通知时，能调用自身的 update() 方法，并触发Compile中绑定的回调，则功成身退。
　　第四步：MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果