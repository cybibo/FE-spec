浏览web文章的时候 收集的大厂的一些面试题 当然是查漏补缺,学习一波了

原文问题链接: https://juejin.im/post/5bc92e9ce51d450e8e777136

—————- 兑吧-微医-寺库-宝宝树 —————-

————————— 兑吧 —————————
localStorage和cookie有什么区别?

CSS选择器有哪些?

盒子模型，以及标准情况和IE下的区别?

如何实现高度自适应?

prototype和——proto——区别?

_construct是什么?

new是怎么实现的 ?

promise的精髓，以及优缺点?

如何实现H5手机端的适配?

rem、flex的区别（root em）?

em和px的区别?

React声明周期?

如何去除url中的#号?

Redux状态管理器和变量挂载到window中有什么区别?

webpack和gulp的优缺点?

如何实现异步加载?

如何实现分模块打包（多入口）?

前端性能优化（1js css；2 图片；3 缓存预加载； 4 SSR； 5 多域名加载；6 负载均衡）?

并发请求资源数上限（6个）?

base64为什么能提升性能，缺点?

介绍webp这个图片文件格式?
WebP是一种有损压缩 这个格式的图像的体积将比JPEG格式图像减小40% 主要优势在于高效率

介绍koa2?

Promise如何实现的?

异步请求，低版本fetch如何低版本适配?

ajax如何处理跨域?

CORS如何设置?

jsonp为什么不支持post方法?
只支持get方法 获取数据

介绍同源策略?

React使用过的一些组件?

介绍Immuable?

介绍下redux整个流程原理?

介绍原型链?

如何继承?

————————— 微医 —————————
介绍JS数据类型，基本数据类型和引用数据类型的区别?

Array是Object类型吗?

数据类型分别存在哪里?

var a = {name: “前端开发”}; var b = a; a = null那么b输出什么
var a = {b: 1}存放在哪里
var a = {b: {c: 1}}存放在哪里

栈和堆的区别?

垃圾回收时栈和堆的区别?

数组里面有10万个数据，取第一个元素和第10万个元素的时间相差多少?
没有差别吧 通过索引index去拿

栈和堆具体怎么存储?

介绍闭包以及闭包为什么没清除?

闭包的使用场景?

JS怎么实现异步?

异步整个执行周期?

Promise的三种状态?

Async/Await怎么实现?

Promise和setTimeout执行先后的区别?

JS为什么要区分微任务和宏任务?

Promise构造函数是同步还是异步执行，then呢?

发布-订阅和观察者模式的区别?

JS执行过程中分为哪些阶段?

词法作用域和this的区别?
一个是定义时的作用域 一个是执行时的作用域

平常是怎么做继承?

深拷贝和浅拷贝?

loadsh深拷贝实现原理?

ES6中let块作用域是怎么实现的?

React中setState后发生了什么?

setState为什么默认是异步?

setState什么时候是同步的?

为什么3大框架出现以后就出现很多native（RN）框架（虚拟DOM）?

虚拟DOM主要做了什么?

虚拟DOM本身是什么（JS对象）

304是什么?
客户端已经执行了GET，但文件未变化

打包时Hash码是怎么生成的?
版本号 + 时间戳

随机值存在一样的情况，如何避免?
多维度产生随机数

使用webpack构建时有无做一些自定义操作?

webpack做了什么?

a，b两个按钮，点击aba，返回顺序可能是baa，如何保证是aba（Promise.all）?

node接口转发有无做什么优化?

node起服务如何保证稳定性，平缓降级，重启等?

RN有没有做热加载?
没有

RN遇到的兼容性问题?

RN如何实现一个原生的组件?

RN混原生和原生混RN有什么不同?

什么是单页项目?

遇到的复杂业务场景?

Promise.all实现原理?

————————— 寺库 —————————
介绍Promise的特性，优缺点?

介绍Redux?

RN的原理，为什么可以同时在安卓和IOS端运行?

RN如何调用原生的一些功能?

介绍RN的缺点?

介绍排序算法和快排原理?
快排原理: 一个数为基准 小的放前面 大的放后面 (再重复)

堆和栈的区别?

介绍闭包?

闭包的核心是什么?

网络的五层模型?

HTTP和HTTPS的区别?

HTTPS的加密过程?

介绍SSL和TLS?
SSL记录协议和SSL握手协议

介绍DNS解析?

JS的继承方法?

介绍垃圾回收?

cookie的引用为了解决什么问题?

cookie和localStorage的区别?

如何解决跨域问题?

前端性能优化?

————————— 宝宝树 —————————
使用canvas绘图时如何组织成通用组件?

formData和原生的ajax有什么区别?
设置header 里的 Content-Type 的类型
Content-type：application/x-www-form-urlencoded

介绍下表单提交，和formData有什么关系?

介绍redux接入流程?

rudux和全局管理有什么区别（数据可控、数据响应）?

RN和原生通信 ?
使用回调函数Callback，它提供了一个函数来把返回值传回给JavaScript
使用Promise来实现
原生模块向JavaScript发送事件

介绍MVP怎么组织?

介绍异步方案?

promise如何实现then处理?

koa2中间件原理?
koa2 的中间件是洋葱模型。基于async/await 可以更好的处理异步操作

常用的中间件?
1.koa:面向node.js的表达式HTTP中间件框架，使Web应用程序和API更加令人愉快地编写
2.koa-router:Router middleware for koa(koa的路由中间件)
3.koa-views:Template rendering middleware for koa.(koa的模板渲染中间件)
4.koa-bodyparser:用来解析body的中间件，比方说你通过post来传递表单，json数据，或者上传文件，在koa中是不容易获取的，通过koa-bodyparser解析之后，在koa中this.body就能直接获取到数据。ps：xml数据没办法通过koa-bodyparser解析，有另一个中间件koa-xml-body
5.promise-mysql:Promise-mysql是mysqljs / mysql的一个包装函数，它包含Bluebird承诺的函数调用。通常这会用Bluebird的.promisifyAll()方法完成，但是mysqljs / mysql的脚本与Bluebird所期望的不同

服务端怎么做统一的状态处理?
用promise拦截状态码

如何对相对路径引用进行优化?

node文件查找优先级?

npm2和npm3+有什么区别?
npm3 和 npm2 对于依赖的处理不一样了
npm2所有项目依赖是嵌套关系，而npm3为了改进嵌套过多、套路过深的情况，会将所有依赖放在第二层依赖中（所有依赖只嵌套一次，彼此平行，也就是平铺的结构