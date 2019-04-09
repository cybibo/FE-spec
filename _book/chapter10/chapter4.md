浏览web文章的时候 收集的大厂的一些面试题 当然是查漏补缺,学习一波了

原文问题链接: https://juejin.im/post/5bc92e9ce51d450e8e777136

—————- 沪江-饿了么-携程-喜马拉雅 —————-

————————— 沪江 —————————
介绍下浏览器跨域?
协议 域名 端口号 的 同源同域策略来确保请求的安全性

怎么去解决跨域问题?

jsonp方案需要服务端怎么配合?
配置返回的数据类型必须是jsonp 类型的，而不是json 类型的,解决跨域必须在ajax 方法中dataType 设置为jsonp
客户端js 代码中ajax 方法还要设置jsonpCallback 这个属性

Ajax发生跨域要设置什么（前端）?
Origin: https://blog.csdn.net

加上CORS之后从发起到请求正式成功的过程?
Request Method: OPTIONS 请求是否能行
Request Method: POST 发起请求

xsrf跨域攻击的安全性问题怎么防范?
是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并执行一些操作（如发邮件，发消息，甚至财产操作如转账和购买商品）
提交验证码: 在表单中增加一个随机的数字或字母验证码，通过强制用户和应用进行交互，来有效地遏制CSRF攻击
Referer Check : 检查如果是非正常页面过来的请求，则极有可能是CSRF攻击
token验证: 在 HTTP 请求中以参数的形式加入一个随机产生的 token，并在服务器端建立一个拦截器来验证这个 token，如果请求中没有 token 或者 token 内容不正确，则认为可能是 CSRF 攻击而拒绝该请求
在 HTTP 头中自定义属性并验证: 类似cookie的方法

使用Async会注意哪些东西?
await 接收返回来的 Promise , 用在 try{}catch{} 中抓取错误信息

Async里面有多个await请求，可以怎么优化（请求是否有依赖）?
await promise.all([p1, p2]) 同时发起异步请求

Promise和Async处理失败的时候有什么区别?
Promise、generator错误都可以在构造体里面被捕获，而async/await返回的是promise,可以通过catch直接捕获错误
generator 抛出的错误，以及await 后接的Promise.reject都必须被捕获，否则会中断执行

Redux在状态管理方面解决了React本身不能解决的问题?
props 层级太深的问题

Redux有没有做过封装?
一份store树，离开页面再次进入，数据不会初始化: (@修饰器) connectstore对ReactDom继承注入action和store，重写componentWillUnmount生命周期，离开页面自动触发store初始化

react生命周期，常用的生命周期?

对应的生命周期做什么事?

遇到性能问题一般在哪个生命周期里解决?
shouldUpdateComponent

怎么做性能优化（异步加载组件…）?
Time slicing upscape

写react有哪些细节可以优化?

React的事件机制（ 绑定到 document ）?
React组件上声明的事件没有绑定在React组件对应的原生DOM节点上，而是绑定在document节点上，触发的事件是对原生事件的包装
主要包含事件注册、触发以及回调函数的存储
ReactEventListener：负责事件注册和事件分发, React将DOM事件全都注册到document节点上，事件分发主要调用dispatchEvent进行，从事件触发组件开始，向父元素遍历
ReactEventEmitter：负责每个组件上事件的执行
EventPluginHub：负责回调函数的存储

介绍下事件代理，主要解决什么问题?

前端开发中用到哪些设计模式?
单例模式: 购物车, 全局缓存
观察者模式: 登录页面登录后，会需要刷新各个模块的信息（头像、nav）这类
工厂模式: 学生 课程 分数
命令模式: 刷新菜单界面
策略模式: 商品打折

React/Redux中哪些功能用到了哪些设计模式?
代理模式: 事件的注册, 触发
@connect: 装饰器模式
一个 store: 单例模式
appmidlleware(thunk, logger): 中介者模式
state => view : 发布订阅者模式

JS变量类型分为几种，区别是什么?
值类型 引用类型

JS里垃圾回收机制是什么，常用的是哪种，怎么处理的?
标记清除法: 函数声明一个变量的时候，就将这个变量标记为“进入环境” 当变量离开环境时，则将其标记为“离开环境” 然后回收
引用计数法: 跟踪记录每个值被引用的次数 引用次数为0的值所占用的内存就会被回收

一般怎么组织CSS（Webpack）
miniExtraCss()
style-loader css-loader postCss-loader less-loader

————————— 饿了么 —————————
小程序里面开页面最多多少?
每个小程序账号仅支持配置最多20个域名
每个域名最多可绑定20个小程序
在公众平台后台域名配置成功后，才可以使用web-view组件

React子父组件之间如何传值?
props func

Emit事件怎么发，需要引入什么?

介绍下React高阶组件，和普通组件有什么区别?
把组件当参数传入函数组件中, 作相应的逻辑处理 再返回

一个对象数组，每个子对象包含一个id和name，React如何渲染出全部的name? map()
在哪个生命周期里写? render()
其中有几个name不存在，通过异步接口获取，如何做? 把对象放在 state 里, 先提示 loading , 获取到了后 再渲染
渲染的时候key给什么值，可以使用index吗，用id好还是index好? id; 数组增删的时候 index 对应的数据存在变化

webpack如何配sass，需要配哪些loader?
sass-loader node-sass

配css需要哪些loader
style-loader css-loader postCss-loader

如何配置把js、css、html单独打包成一个文件?
js => bundle.js
css => miniCss => cope.js
html => router.page

div垂直水平居中（flex、绝对定位）?

两个元素块，一左一右，中间相距10像素,上下固定，中间滚动布局如何实现?

[1, 2, 3, 4, 5]变成[1, 2, 3, a, b, 5]?

1
2
3
const index = array.findIndex(4)
const arrayBouth =  array.slice(index, 1)
return [...arrayBouth[0], ...[a, b], ...arrayBouth[1]]
取数组的最大值（ES5、ES6）?
Math.max(…[1,2,3])

apply和call的区别?

ES5和ES6有什么区别?

some、every、find、filter、map、forEach有什么区别?

上述数组随机取数，每次返回的值都不一样?
Math.random()

如何找0-5的随机数，95-99呢?

1
array.filter(item => item > 0 && item < 5)
页面上有1万个button如何绑定事件?
事件委托, 冒泡触发

如何判断是button?
冒泡的时候带上 index

页面上生成一万个button，并且绑定事件，如何做（JS原生操作DOM）

循环绑定时的index是多少，为什么，怎么解决?
9999 0 开始

页面上有一个input，还有一个p标签，改变input后p标签就跟着变化，如何处理?
监听input的哪个事件，在什么时候触发
onKeyup

————————— 携程 —————————
对React看法，有没有遇到一些坑?
pureComponent 只是浅比较
以及一些多次渲染的 render

对闭包的看法，为什么要用闭包?
增加了内存的消耗
某些浏览器上因为回收机制的问题，有内存溢出风险
增加了代码的复杂度，维护和调试不便

延长作用域链
生成预编译函数
更好的组织代码，比如模块化，异步代码转同步
setTimeout()(i) 问题

手写数组去重函数?
new Set(arr) (可能有兼容性问题)
arr.sort() 不相等 就push到新的数组中
利用对象的属性不能相同的特点进行去重
利用includes

手写数组扁平化函数?
就是把多维数组变成一维数组

1
2
3
4
5
6
7
function flatten(arr) {
    return arr.join(',').split(',').map(function(item) {
        return parseInt(item);
    })
}

[].concat(...[1, 2, 3, [4, 5]]);  // [1, 2, 3, 4, 5]
介绍下Promise的用途和性质?

Promise和Callback有什么区别?

React生命周期?

————————— 喜马拉雅 —————————
ES6新的特性?

介绍Promise?

Promise有几个状态?

说一下闭包?

React的生命周期?

componentWillReceiveProps的触发条件是什么?

React16.3对生命周期的改变?
UNSAFE_componentWillMount
UNSAFE_componentWillReceiveProps
UNSAFE_componentWillUpdate

介绍下React的Filber架构?
改变了之前react的组件渲染机制，新的架构使原来同步渲染的组件现在可以异步化，可中途中断渲染，执行更高优先级的任务。释放浏览器主线程
画Filber渲染树?
就是组件的生命周期树, componentWillMount 一层一层的往下渲染 componentWillUnmount再往上

介绍React高阶组件?

父子组件之间如何通信?

Redux怎么实现属性传递，介绍下原理?

React-Router版本号?

网站SEO怎么处理?
前端加 meta discription keyword
后端 ssr

介绍下HTTP状态码 403、301、302是什么?
403 服务器拒绝请求
301 永久重定向
302 临时重定向

缓存相关的HTTP请求头?
尽量降低http的请求次数
Expires
Cache-Contro: Public Private no-cache no-store max-age min-fresh
Last-Modified/If-Modified-Since
ETag

no-cache>Expires>Last-Modified 前面的生效后,后面的基本就失效了

介绍HTTPS ?

HTTPS怎么建立安全通道?

前端性能优化（JS原生和React）?

用户体验做过什么优化?

对PWA有什么了解?
progressive web app: 渐进式网页应用
客户端要和推送服务进行绑定，会生成一个绑定后的推送服务API接口，服务端调用此接口，发送消息。同时，浏览器也要支持推送功能，在注册 sw 时, 加上推送功能的判断

对安全有什么了解?

介绍下数字签名的原理?
A：将明文进行摘要运算后得到摘要（消息完整性），再将摘要用A的私钥加密（身份认证），得到数字签名，将密文和数字签名一块发给B。
B：收到A的消息后，先将密文用自己的私钥解密，得到明文。将数字签名用A的公钥进行解密后，得到正确的摘要（解密成功说明A的身份被认证了

前后端通信使用什么方案?

RESTful常用的Method?

介绍下跨域?

Access-Control-Allow-Origin在服务端哪里配置?
response的header

csrf跨站攻击怎么解决?
token post方法 http header 中加验证

前端和后端怎么联调?