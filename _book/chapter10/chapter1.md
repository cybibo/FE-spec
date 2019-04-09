搜集的一些面试题, 现在越发的觉得( JavaScript高级程序设计 和 ES6标准入门(第3版) 很重要, 抽时间会再通读一遍)
有点零散,含括了大部分面试点

0、promise iterator(for of) generator async await
手写依赖注入
用promise封装个东西
前端架构的东西

1、css3动画性能问题，怎么优化

是否导致layout,尽可能将动画元素absolute或者fixed化以避免影响文档树，以减少重排;
是否启用硬件加速,开启硬件加速在webkit中有神奇的万金油：opacity: 1;或者-webkit-backface-visibility: hidden;
是否是有高消耗的属性,（css shadow、gradients、background-attachment: fixed等）,有的话，图片也是一种选择。这算得上是用空间换时间的优化了。
repaint的面积, 如果是，只好缩小动画面积了。这一步的优化有限;
尽量使用 transform 生成动画，避免使用 height,width,top,left,margin,padding 等, 使用 transform，浏览器只需要一次生成这个元素的位图，并在动画开始的时候将它提交给 GPU 去处理
(用canvas、css3、jquery制作动画.在移动端动画效果上，使用css3动画要比jquery动画效率高的多。在安卓手机上表现尤其明显.所以移动端动画以css3动画为优先，jquery只能用来简单处理应用逻辑.css3动画是用来给内容布局加上特效的通用解决方案，但是在性能堪忧的移动浏览器上很可能会受排版性能所限，达不到理想的效果。而对性能有要求的特定场景，比如游戏，用canvas会有很大的提高.)
2、flex布局
( flex-direction | flex-wrap ) => flex-flow
justify-content | align-items
order | flex-grow | flex-shrink | flex-basis => flex | align-self
3、输入url到界面渲染发生了什么
大致分为两个部分：网络通信和页面渲染
互联网内各网络设备间的通信都遵循TCP/IP协议 应用层、传输层、网络层、数据链路层 物理层 在浏览器中输入url => 应用层DNS解析域名 => 应用层客户端发送HTTP请求 => 传输层TCP传输报文(三次握手) => 数据到达数据链路层 => 服务器接收数据,服务器响应请求
DOM树 + CSS树 => render树 => 在遇到外部链入的脚本标签或样式标签或图片时，会再次发送HTTP请求重复上述的步骤
4、手写diff算法
5、手写依赖注入
通过正则表达式来提取function中的参数列表
根据参数列表寻找依赖 取得了参数列表，就等于取得了依赖列表，剩下的工作就是将依赖当做参数传入
传递依赖项参数并实例化
var Injector = function(){
this._cache = {};
}
Injector.prototype.getParamNames = function(func){
var paramNames = func.toString().match(/function\s+[^(](([^)]))/m)[1];
paramNames = paramNames.replace(/\s*/,””);
paramNames = paramNames.split(“,”);
return paramNames;
}
Injector.prototype.put = function(name,obj){
this._cache[name] = obj;
}
Injector.prototype.resolve = function(func,bind){
var paramNames = this.getParamNames(func),params = [];
for(var i=0,l=paramNames.length;i<l;i++){
params.push(this._cache[paramNames[i]]);
}
func.apply(bind,params);
}

6、继承、原型链作用域链

原型链上去查找属性和方法 全局作用域==>函数作用域
Function.prototype = {
constructor : Function,
proto : parent prototype,
some prototype properties: …
};
7、单线程 异步线程执行机制 deferred promise、async await fetch

js是单线程的，浏览器是多线程的，其异步也得靠其他线程来监听事件的响应，并将回调函数推入到任务队列等待执行。单线程所做的就是执行栈中的同步任务，执行完毕后，再从任务队列中取出一个事件（没有事件的话，就等待事件），然后开始执行栈中相关的同步任务，不断的这样循环
deferred
function runAsync(){
var def = $.Deferred();
//做一些异步操作
setTimeout(function(){//模拟异步操作
def.resolve('随便什么数据');
}, 2000);
return def;
}
runAsync().then(function(data){
console.log(data)
});
async await
var stop= function (time) {
return new Promise(function (resolve, reject) {
setTimeout(function () {
    resolve();
}, time);
})
};
var start = async function () {
// 在这里使用起来就像同步代码那样直观
console.log(‘start’);
try{
await stop(3000);
}catch(err){
console.log(err);
}
console.log(‘end’);
};
start();

Promise.all Promise.race
let wake = (time) => {
return new Promise((resolve, reject) => {
setTimeout(() => {
resolve(`${time / 1000}秒后醒来`)
}, time)
})
}
let p1 = wake(3000)
let p2 = wake(2000)

Promise.all([p1, p2]).then((result) => {
console.log(result) // [ ‘3秒后醒来’, ‘2秒后醒来’ ]
}).catch((error) => {
console.log(error)
})

8、event loop

主线程从”任务队列”中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为Event Loop（事件循环）
process.nextTick方法指定的回调函数，总是在当前”执行栈”的尾部触发
9、说说你对前端架构的理解
前端架构是一系列工具和流程的集合,旨在提神前端代码的质量,并实现高效可持续的工作流.(代码/流程/测试/文档)
10、回流 重绘 bfc
回流 (重新渲染) 重绘 (简单的修改)
放到其他图层中的优点就是：当一个元素在自己图层内发生变化的时候它的回流和重绘只会在图层内部发生，从而减少了浏览器对于重绘与回流的运算量。
用transform 代替 top，left ，margin-top， margin-left… 这些位移属性
不要使用 js 代码对dom 元素设置多条样式，选择用一个 className 代替之
动画的速度按照业务按需决定
对于频繁变化的元素应该为其加一个 transform 属性，对于视频使用video 标签
必要时可以开启 GPU 加速，但是不能滥用
11、react的组件类型（class、functional、受控组建、非受控组建）的区别
非受控组建 用默认值展示
受控组建 组件的状态变化受到state的控制
无状态组件 functional 若一个组件不含有状态和对状态的处理，则可以将render方法单独抽取出来，成为一个独立的组件函数 无状态组件转化为有状态组件，则通过高阶组件转化；方式就是高阶组件通过props传入state
高阶组件 一个包含了另一个React组件的React组件；本质上就是一个函数 (属性代理/反向代理)
function HOC(Com) {
// 其他处理
return class [Name] extends Component {
constructor(props) {
super(props);
}
render() {
return (
  <div>
     <Com {...this.props} />
  </div>
)
}
}
}
function Text(props) {
return (
<p>{props.name}</p>

)
}
const H = HOC(Text)
function HOC(B){
return class [A] extends B{
render(){
return super.render();
}
}
12、css边距塌陷以及解决办法

相邻元素之间（上下相邻会塌陷，左右不会塌陷） 子元素紧贴父元素的最外边时，子元素的margin外边距会影响父元素 空的块元素中没有任何东西，则上下外边距折叠
BFC是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面元素
13、前端性能优化（记不清怎么问的了）
请减少HTTP请求基本原理
请正确理解 Reflow 和 Repaint
请减少对DOM的操作
使用JSON格式来进行数据交换
使用CDN加速（内容分发网络）
CSS和JavaScript的压缩，直接减少下载的文件体积
压缩图片和使用图片Sprite技术
注意控制Cookie大小和污染
14、检查是否为空对象
使用Object对象的getOwnPropertyNames方法，获取到对象中的属性名，存到一个数组中，返回数组对象，我们可以通过判断数组的length来判断此对象是否为空
var data = {};
var arr = Object.getOwnPropertyNames(data);
alert(arr.length == 0);//true
使用ES6的Object.keys()方法
与4方法类似，是ES6的新方法, 返回值也是对象中属性名组成的数组
var data = {};
var arr = Object.keys(data);
alert(arr.length == 0);//true
15、数组删除某一个元素
splice
16、react 手动 UNmount
操作数据就好,让组件自己去销毁
17、函数的几种命名方式
function name(){}
var func = fun
() => {}
18、vue的理解 事件周期 vuex 异步处理 diff算法
响应式 Object.defineProperty(obj, ‘name’, { get:()=> {}, set: () => {} })
模板引擎 with(this) { return _c() }
渲染 updateComponent
beforeCreate/created boforeMount/mounted beforeUpdate/updated beforeDestroy/destroyed
vuex
diff算法
vm._update(vnode) {
const prevVnode = vm._vnode
vm._vnode = vnode
if (!prevVnode) {
vm.$el = vm.__patch__(vm.$el,vnode)
} else {
vm.$el = vm.__patch__(prevVnode,vnode)
}
}
function updateComponent () {
vm._update(vm._render())
}
19、react的理解 事件周期 redux
组件化 (封装、复用), 单向props
componentWillMount/componentDidMount componentWillReceiveProps shouldComponentUpdate componentWillUpdate/componentUpdated componentWillUnmount
-redux
20、浏览器缓存 cache-control max-age no-modifail e-tag

Expire HTTP1.0
Cache-Control: max-age=0
Last-Modified 304
If-Modified-Since 如果If-Modified-Since的时间比服务器当前时间还晚，会认为是个非法请求
Etag 服务器响应时给请求URL标记，并在HTTP响应头中将其传送到客户端
21、设计模式 及其使用
工厂模式 单例模式 适配器模式 装饰器模式 代理模式(new Proxy(star, {get:function () {},set:function () {} }))
外观模式 (绑定事件) 观察者模式 （发布/订阅 一对N） 迭代器模式 状态模式 策略模式
class Subject {
constructor () {
this.state = 0
this.observers = []
}
getState () {
return this.state
}
setState (state) {
this.state = state
this.notifyAllObservers()
}
notifyAllObservers () {
this.observers.forEach(observer => {
    observer.update()
})
}
attach (observer) {
this.observers.push(observer)
}
}
class observer {
constructor (name, subject) {
this.name = name
this.subject = subject
this.subject.attach(this)
}
update () {
console.log(${this.name} update, state: ${this.subject.getState()})
}
}
// 测试代码
let s = new Subject()
let o1 = new observer(‘o1’, s)
let o2 = new observer(‘o2’, s)
let o3 = new observer(‘o3’, s)

s.setState(1)
s.setState(2)

22、apply call bind(this)
23、itorator for of
class Iterator {
constructor (container) {
this.list = container.list
this.index = 0
}
next () {
if (this.hasNext()) {
return this.list[this.index++]
}
return null
}
hasNext () {
if (this.index >= this.list.length) {
return false
}
return true
}
}

class Container {
constructor (list) {
this.list = list
}
getIterator () {
return new Iterator(this)
}
}

let container = new Container([1, 2, 3, 4, 5])
let iterator = container.getIterator()
while (iterator.hasNext()) {
console.log(iterator.next())
}

24、项目的目录结构及作用 (src)

components | config | constants | containers | decorator | imgs | mock | redux | services | styles | utils
favicon | index.html | index.js | router.js | store.js
25 异步事件的执行顺序
script(主程序代码)—>process.nextTick—>Promises…——>setTimeout——>setInterval——>setImmediate——> I/O——>UI rendering
26 vuex redux 对比 : VUEX与React-Redux：一个是针对VUE优化的状态管理系统，一个仅是常规的状态管理系统（Redux）
— vue

index.js
import Vue from ‘vue’
import Vuex from ‘vuex’
import as actions from ‘./actions’
import as getters from ‘./getters’
import state from ‘./state’
import mutations from ‘./mutations’
import createLogger from ‘vuex/dist/logger’
Vue.use(Vuex)

const debug = process.env.NODE_ENV !== ‘production’

export default new Vuex.Store({
actions,
getters,
state,
mutations,
strict: debug,
plugins: debug ? [createLogger()] : []
})

actions
getters
export const language = state => state.language
export const client = state => state.client
export const routerTo = state => state.routerTo
mutation-types
export const LANGUAGE = ‘LANGUAGE’
export const CLIENT = ‘CLIENT’
export const ROUTERTO = ‘ROUTERTO’
mutations
import * as types from ‘./mutation-types’
const mutations = {
types.LANGUAGE {
state.language = language
},
types.CLIENT {
state.client = client
},
types.ROUTERTO {
state.routerTo = routerTo
}
}
export default mutations
state
const state = {
language: ‘chinese’,
client: ‘pc’,
routerTo: ‘home’
}
export default state
import {mapGetters} from ‘vuex’
computed: {
…mapGetters([
‘language’
])
},

— redux

store(index.js)
import { createStore, applyMiddleware, compose } from ‘redux’
import thunk from ‘redux-thunk’
import reducer from ‘./reducer’
const composeEnhancers = window.REDUX_DEVTOOLS_EXTENSION_COMPOSE || compose;
const store = createStore(reducer, / preloadedState, / composeEnhancers(
applyMiddleware(thunk)
));
export default store
(store)reducer.js
import { combineReducers } from ‘redux-immutable’
import { reducer as headerReducer } from ‘../common/header/store’
import { reducer as homeReducer } from ‘../pages/home/store’
import { reducer as detailReducer } from ‘../pages/detail/store’
import { reducer as loginReducer } from ‘../pages/login/store’
const reducer = combineReducers({
header: headerReducer,
home: homeReducer,
detail: detailReducer,
login: loginReducer
})
export default reducer
(header/store) index.js
import reducer from ‘./reducer’
import as actionCreators from ‘./actionCreators’
import as actionTypes from ‘./actionTypes’
export { reducer, actionCreators, actionTypes }
(header/store) reducer.js
import * as actionTypes from ‘./actionTypes’
import { fromJS } from ‘immutable’
const defaultState = fromJS({
focused: false,
mouseIn: false,
list: [],
page: 1,
totalPage:1
})
export default (state = defaultState, action) => {
switch (action.type) {
case actionTypes.SEARCH_FOCUS :
    return state.set('focused', true)
case actionTypes.SEARCH_BLUR:
    return state.set('focused', false)
case actionTypes.CHANGE_LIST:
    return state.merge({
        list: action.data,
        totalPage: action.totalPage
    })
case actionTypes.MOUSE_ENTER:
    return state.set('mouseIn', true)
case actionTypes.MOUSE_LEAVE:
    return state.set('mouseIn', false)
case actionTypes.CHANGE_PAGE:
return state.set('page', action.nextPage)
default:
    return state
}
}
actionTypes.js
export const SEARCH_FOCUS = ‘header/SEARCH_FOCUS’
export const SEARCH_BLUR = ‘header/SEARCH_BLUR’
export const CHANGE_LIST = ‘header/CHANGE_LIST’
export const MOUSE_ENTER = ‘header/MOUSE_ENTER’
export const MOUSE_LEAVE = ‘header/MOUSE_LEAVE’
export const CHANGE_PAGE = ‘header/CHANGE_PAGE’
actionCreators.js
import * as actionTypes from ‘./actionTypes’
import axios from ‘axios’
import { fromJS } from ‘immutable’
export const searchFocus = () => ({
type: actionTypes.SEARCH_FOCUS
})
export const searchBlur = () => ({
type: actionTypes.SEARCH_BLUR
})
const changeList = (data) => ({
type: actionTypes.CHANGE_LIST,
data: fromJS(data) , // 数据类型 保持一致 都为immutable数组
totalPage: Math.ceil(data.length / 10)
})
export const getList = () => {
return (dispatch) => {
axios.get('/api/headerList.json').then((res) => {
    if (res.data.success) {
        dispatch(changeList(res.data.data))
    }
}).catch( (e) => {
    console.log(e)
})
}
}
export const mouseEnter = () => ({
type: actionTypes.MOUSE_ENTER
})
export const mouseLeave = () => ({
type: actionTypes.MOUSE_LEAVE
})

export const changePage = (nextPage) => ({
type: actionTypes.CHANGE_PAGE,
nextPage
})

header index.js
import { connect } from ‘react-redux’;
import { actionCreators } from ‘./store’;
import { actionCreators as loginActionCreators } from ‘../../pages/login/store’
const mapStateToProps = (state) => ({
focused: state.getIn([‘header’, ‘focused’]),
// state.get(‘header’).get(‘focused’)
list: state.getIn([‘header’, ‘list’]),
page: state.getIn([‘header’, ‘page’]),
totalPage: state.getIn([‘header’, ‘totalPage’]),
mouseIn: state.getIn([‘header’, ‘mouseIn’]),
login: state.getIn([‘login’, ‘login’])
})
const mapDispatchToProps = (dispatch) => {
return {
handleInputFocus (list) {
(!list.length) && dispatch(actionCreators.getList())
dispatch(actionCreators.searchFocus())
},
handleInputBlur () {
dispatch(actionCreators.searchBlur())
},
handleMouseIn () {
dispatch(actionCreators.mouseEnter())
},
handleMouseLeave () {
dispatch(actionCreators.mouseLeave())
},
handleChangePage (page, totalPage) {
let nextPage
if (page < totalPage) {
nextPage = page + 1
} else {
nextPage = 1
}
dispatch(actionCreators.changePage(nextPage))
},
logout() {
dispatch(loginActionCreators.logout())
}
}
}
export default connect(mapStateToProps, mapDispatchToProps)(Header)

const { focused, handleInputFocus, handleInputBlur, list, login, logout } = this.props

跨域实现
1、 通过jsonp跨域
2、 document.domain + iframe跨域
3、 location.hash + iframe
4、 window.name + iframe跨域
5、 postMessage跨域
6、 跨域资源共享（CORS）
7、 nginx代理跨域
8、 nodejs中间件代理跨域
9、 WebSocket协议跨域

web安全
Content-Security-Policy: ‘http: https: domain:’
Set-cookie: ‘id=123 httpOnly’
HTTPS
xss
crsf

用CSS实现一个三角形
div {
width: 0;
height: 0;
border-width: 40px;
border-style: solid;
border-color: red transparent transparent transparent;
}

判断是对象还是数组的方法
typeOf
Array.isArray(arr) arr
Array.isArray(obj) obj

for…in 和for…of的区别 遍历数组和对象
for…in遍历得到的是 key:value 中的key 数组的话 就是 返回数组的index值
for…of 遍历里面的可以value

同源不同标签之间的通信 1使用websocket协议、2通过localstorage、3以及使用html5浏览器的新特性SharedWorker。

fetch和ajax的区别
1、fetch()返回的promise将不会拒绝http的错误状态，即使响应是一个HTTP 404或者500。 服务器端返回 状态码 400 500的时候 不会reject Response.ok 属性为 true。 2、在默认情况下 fetch不会接受或者发送cookies ，要发送 cookies，必须设置 credentials 选项。 credentials: ‘include’

localStorage和sessionStorage的区别
保存数据的生命周期不同。 sessionStorage只是可以将一部分数据在当前会话中保存下来，刷新页面数据依旧存在。但当页面关闭后，sessionStorage 中的数据就会被清空

针对移动浏览器端开发页面，不期望用户放大屏幕，且要求“视口（viewport）”宽度等于屏幕宽度，视口高度等于设备高度，如何设置？

data-xxx 属性的作用是什么？
data- 属性赋予我们在所有 HTML 元素上嵌入自定义 data 属性的能力。 存储的（自定义）数据能够被页面的 JavaScript 中利用，以创建更好的用户体验

请描述一下cookies，sessionStorage和localStorage的区别？
cookie:一般由服务器生成，可设置失效时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效，也可设置过期时间,每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题 sessionStorage: 仅在当前会话下有效，关闭页面或浏览器后被清除 localStorage: 除非被清除，否则永久保存,仅在客户端（即浏览器）中保存，不参与和服务器的通信

什么是浏览器的标准模式和怪异模式
1） 盒模型： IE下标准模式为标准w3c盒模型【content+padding+border+margin】，怪异模式为IE盒模型【content+margin：padding与border包含在content宽高中
2） 行内元素的垂直对齐：基于 Gecko 的浏览器【Mozilla Firefox、HotBrowser、Mozilla Suite、Camino】标准模式对齐至基线，怪异模式对齐至底部
3） 怪异模式中，IE6/7/8都不认识!important声明
4） 设置行内元素的高宽： 在Standards模式下，给等行内元素设置wdith和height都不会生效，而在quirks模式下，则会生效
5） 使用margin:0 auto在standards模式下可以使元素水平居中，但在quirks模式下却会失效。

解释一下盒模型宽高值的计算方式，边界塌陷，负值作用，box-sizing概念
盒模型 a. ie678怪异模式（不添加 doctype）使用 ie 盒模型，宽度=边框+padding+内容宽度 b. chrome， ie9+, ie678(添加 doctype) 使用标准盒模型，宽度= 内容宽度
边界塌陷 同一BFC下，相邻快级元素边界取最大值 负值作用 display:inline-block是什么呢？相信大家对这个属性并不陌生，根据名字inline-block我们就可以大概猜出它是结合了inline和block两者的特性于一身，简单的说：设置了inline-block属性的元素既拥有了block元素可以设置width和height的特性，又保持了inline元素不换行的特性

1重绘和回流的区别

2视图属性 clientX，pageX，offsetX，screenX

清除浮动:
.clearfix:before,.clearfix:after {
display: table;
content: “ “
}

.clearfix:after {
clear: both
}

5、两列布局（右侧定宽200px,左边随屏幕自动填充宽度）

#sidebar, #container {min-height:400px; / 视具体情况 /}

#sidebar {width:210px; background-color:#ebb058; position:fixed; top:0; left:0;}

#container {margin-left:210px; border:2px solid #400080;}

7、addEventListener()有几个参数，是什么？
element.addEventListener(event, function, useCapture)
第一个参数是事件的类型 (如 “click” ).
第二个参数是事件触发后调用的函数
第三个参数是个布尔值。默认是false（冒泡阶段执行）true(捕获阶段产生)

8.手写一个闭包
Let singeModel = () => {
Let instance
if (! instance) {
return new singeModel()
}
return instance
}

apply、call、bind区别、用法
都是用来改变函数的this对象的指向的；
第一个参数都是this要指向的对象；
都可以利用后续参数传参；
bind是返回对应函数，便于稍后调用，apply、call是立即调用；

算法能力如何?
给一个数组如：[[“a”,”b”,”c”],[“d”,”e”],…..]得到[ad,ae,bd,be,cd,ce]，手写实现的方法？（要求js实现） 如何将上面的改成函数式编程风格？ 如果数组中出现[[“a”,”b”,”c”],[“a”,”d”]]要求去掉”aa”这种情况（即两组所取的元素不能有相同的）？不能用filter

跳台阶问题？m阶楼梯，一次最多可跳4次，有多少种可能？（本来问n次，然后直接举例说4次）手写实现代码？
m%n === 0 ? m%n | (m%n + 1)

死锁的条件是什么？
（1）互斥（mutualexclusion），一个资源每次只能被一个进程使用
（2）不可抢占（nopreemption），进程已获得的资源，在未使用完之前，不能强行剥夺
（3）占有并等待（hold andwait），一个进程因请求资源而阻塞时，对已获得的资源保持不放
（4）环形等待（circularwait），若干进程之间形成一种首尾相接的循环等待资源关系

hybrid通信的实现原理
1.H5向Native通信
H5打开一个原生界面，H5只作为一个入口，后续逻辑均在原生界面处理 (原生已封装实现此功能，可供H5直接调用，不必重复造轮子，节省H5开发成本)
H5唤起一个原生界面执行一些逻辑，需要将结果返回给H5
2.Native向H5通信
通常表现原生界面将某些结果传给H5页面或触发某些H5的行为 (通过WebView去加载JavaScript的方法或回调函数/Native容器（Activity或ViewController（iOS））向H5暴露生命周期状态)
3.H5页面之间通信
如果H5项目有多个独立页面，页面之间可能需要通信 (在实现上应抽象封装一个公共的WebView，统一接收H5事件注册及事件触发通知)

给定一个数组arr，选出 n 个数的和等于m 答穷举之法
function f($n, $arr) { //$n是目标数字，$arr是数字组成的数组
if (empty($arr)) return false; //如果数组没有元素，返回
if (in_array($n, $arr)) return [$n];//如果期望的值已经存在，直接返回这个值
foreach($arr as $k => $v) { //遍历数组
if ($v > $n) continue; //比指定数还大，过
$copy = $arr; //复制数组
unset($copy[$k]); //去掉复制数组中已经被选中的数字
$next = f($n - $v, $copy); //递归计算
if(! empty($next)) return array_merge([$v], $next); //合并结果集
}
return false;//没找到啊
}
$arr = [99.1, 92.2, 60, 50, 49.5, 45.7, 25.1, 20, 7.4, 13, 10, 7, 2.1, 2, 1];
$data = f(100, $arr);
print_r($data);//Array ( [0] => 60 [1] => 20 [2] => 13 [3] => 7 )
$data = f(105, $arr);
print_r($data);//Array ( [0] => 60 [1] => 20 [2] => 13 [3] => 10 [4] => 2 )

Bom是什么？列举你知道的Bom对象 BOM是browser object model的缩写，简称浏览器对象模型 ，提供了独立于内容而与浏览器窗口进行交互的对象
window screen location navigator history

get post区别 : get更快，参数在Url里；post数据量无限，安全

web语义化 h1~h6 ul,ol,li dl,dt,dd em,strong header,footer,aside,section,article address

行内元素: span a b strong img i em del u input textarea select
块元素: div form table ul ol dl p h1

三栏布局 : 双飞翼布局 圣杯布局

css选择器优先级 : !important > 元素内的 style 属性 (1000) > id (100) > class (10) > 标签 (1) > 通配符 > 继承 > 浏览器默认属性

https请求过程
HTTPS : HTTPS在HTTP的基础上加入了SSL协议，SSL依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密
公钥私钥
client server
Random-client+支持的加密套件
Random-server+一种加密套件
生成预主密钥 用自己的私钥解密预主密钥
主密钥 —— 主密钥

HTTPS和HTTP的区别主要如下：
优点:
　　1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。
　　2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
　　3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
　　4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。
缺点:
（1）HTTPS协议握手阶段比较费时，会使页面的加载时间延长近50%，增加10%到20%的耗电；
（2）HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗，甚至已有的安全措施也会因此而受到影响；
（3）SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。
（4）SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗。
（5）HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。最关键的，SSL证书的信用链体系并不安全，特别是在某些国家可以控制CA根证书的情况下，中间人攻击一样可行

http2.0 : (二进制分帧传输/头信息压缩/提高推送效能/信道复用/Server Push)

箭头函数和普通函数区别 : this指向、不能做构造函数、不能使用arguments

自我介绍

获取页面上个数top3标签:
var tags = document.getElementsByTagName(‘*’);
var tagsArr = [];
function countTag(){
for (var i = 0; i < tags.length; i++) {
tagsArr.push((tags[i].tagName).toLowerCase());
}
// console.log(tagsArr);
//定义一个数组用于存放相同的元素
var temp = [];
//定义一个空数组用于存放每一个标签；
var tag =[];
for (var i = 0; i < tagsArr.length; i++) {
for (var j = i+1; j < tagsArr.length+1; j++) {
if (tagsArr[i] == tagsArr[j]) {
temp.push(tagsArr[j]);
tagsArr.splice(j,1);
j–;
}
if (j == tagsArr.length -i) {
temp.push(tagsArr[i]);
tagsArr.splice(i,1);
i–;
tag.push(temp);
temp = [];
}
}
}
console.log(tag)
return tag;
}

获取一个页面上使用最多的标签
new Set([…document.querySelectorAll(‘*’)].map(v => v.nodeName)).size

提取url键值对
function getRequest1(){
var str=location.search; // ?userName=zhanghao$userId=123
if(str){
var url=str.substr(1),
arr=url.split(‘&’),
request={};
// [“userName=zhanghao”, “userId=123”]
for(i= 0; i < arr.length; i++){
let temp = arr[i].split(‘=’)
request[temp[0]] = temp[1];
// {userName : ‘zhanghao’, userId : ‘123’}
}
return request; //{userName: “zhanghao”, userId: “123”}
}else{
alert(‘没有传递参数’);
}
}

linux bash命令cp/rm/mv/cat/ln -s/alias
cp — 复制文件和目录
rm — 删除文件和目录
mv — 移动/重命名文件和目录
cat： 查看文件的内容
mkdir — 创建目录
touch 创建文件 或者 修改文件信息
ln — 创建硬链接和符号链接
pwd：查看当前目录
有一个目录很深，如何很快的进入(建立软链接、设置别名？)
-s 生成概况
alias – 创建命令别名

Rem : 用rem就是让在任何分辨率的情况下,宽高比率都是一样。常用的地方就是对图片的处理,防止图片的变形

vm/vh(view-width, view-height)
视区宽度/高度为100vw/100vh

前端实现方案:淘宝手淘:
动态改写标签
给元素添加data-dpr属性,并且动态改写data-dpr的值
给元素添加font-size属性,并且动态改写font-size的值
淘宝手淘团队h5页面终端适配开源库:lib-flexible

小结 :
对于多倍屏,通过rem为单位,viewport: scale=1/dpr来达到适合的显示;
使用iPhone6(375pt, 750px)二倍设计图:750px为基准;
切图使用三倍精度图,以适应三倍屏
css单位综合使用:适配元素rem比例显示,正文字体宜用px+dpr缩放
配合scss函数,简化px2rem转换,且易于维护(若需修改$base-font-size, 无需手动重新计算所有rem单位)

ajax跨域（cors、反向代理）
Content-Type(只限于三个值application/x-www-form-urlencoded、 multipart/form-data、text/plain)
Access-Control-Allow-Headers: X-Requested-With,Content-Type,Accept
Access-Control-Allow-Methods: Get,Post,Put,OPTIONS
Access-Control-Allow-Origin: *

简述jsonp过程
ajax请求过程
(1)创建XMLHttpRequest对象,也就是创建一个异步调用对象.
(2)创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息.
(3)设置响应HTTP请求状态变化的函数.
(4)发送HTTP请求.
(5)获取异步调用返回的数据.
(6)使用JavaScript和DOM实现局部刷新

离线缓存(manifest、service worker

客户端存储方式及异同
Cookie
sessionStorage
localStorage

HTTP1.0 HTTP1.1(持久链接 pipeline host) HTTP2(二进制传输/头信息压缩/提高推送效能) HTTPS的实现:

1.应用层 : HTTP FTP

2.传输层 : TCP协议(更可靠) UDP协议

3.网络层 : 为数据在结点之间传输创建逻辑链路

4.数据链路层 : 在通信的实体间建立数据链路链接

4.物理层 : 定义物理设备如何传输数据

3次握手
client server

同步序列编号SYN=1,Seq=x
SYN=1,确认字符ACK=X+1,Seq=Y
ACK=Y+1,Seq=Z
URI URL URN
URI 用来标识互联网上的信息资源 包含(URL URN)
URL 统一资源定位器

跨域
response.writeHead(200, {
‘Access-Control-Allow-Origin’: ‘*’,
‘Access-Control-Allow-Headers’: ‘X-Test-Cors’,
‘Access-Control-Allow-Methods’: ‘POST, PUT, Delete’,
‘Access-Control-Max-Age’: ‘1000’,
})
-

CORS 预请求
这几个不用预请求
get header post
Content-Type: ‘text/plain’ ‘multipart/form-data’ ‘application/x-www-form-urlencoded’

http 浏览器缓存
可缓存性: Cache-Control: public(任何结点) private(当前浏览器) no-cache(均不能缓存)
Cache-Control: ‘max-age=200, public’ ‘max-age=200, no-cache’ ‘max-age=200, no-store’

缓存验证:
Last-Modified : 对比上次修改时间,验证是否已经被修改 配合 If-Modified-Since If-Unmodified-Since
Etag: 对比资源的数据签名(如:hash),判断是否使用缓存

Cookie
set-cookie : max-age | expired 设置过期时间 | Secure 只在 HTTPS 的时候发送 | HTTPOnly 无法通过document.cookie 访问
‘set-cookie’: [‘id=123;max-age=2’, ‘abc=456; HTTPOnly’, ‘def=678; domain=test.com’]

HTTP长连接
Connection: Keep-Alive | close

数据协商 请求和相应协商数据传输的信息 Accept <> Content
Request Headers
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,/;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36

Response Headers
Content-Type: text/html;charset=utf-8
Content-Encoding: gzip
Content-Language

Redirect 302 临时重定向 301 永久重定向
if (request.url === ‘/‘) {
response.writeHead(302, {
‘Location’: ‘/new’
})
}

Content-Security-Policy 内容安全策略CSP 现在src访问的地址
response.writeHead(302, {
‘Content-Security-Policy’: ‘default-src http: ; https: ; form-action \’self\’ ; ‘
})

HTTPS : HTTPS在HTTP的基础上加入了SSL协议，SSL依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密
公钥私钥
client server
Random-client+支持的加密套件
Random-server+一种加密套件
生成预主密钥 用自己的私钥解密预主密钥
主密钥 —— 主密钥

HTTP2 二进制分帧传输/信道复用/Server Push

vue2.0 响应式的基本思路 (订阅/观察者 模式)
第一步：将data下面的属性变为observable,使用Object.defineProperty对数据对象做属性get和set的监听,当有数据读取和赋值操作时则调用节点的指令，这样使用最通用的=等号赋值就可以触发了。//数据劫持，监控数据变化
第二步：实现一个消息订阅器,我们维护一个数组，这个数组，就放订阅者，一旦触发notify，订阅者就调用自己的update方法
第三步：实现一个 Watcher,就是执行数据变化时我们要执行的操作 (依赖收集追踪原理)
第四步：touch拿到依赖,视图的更新需要哪些数据的支持，并把它记录为数据的订阅者
最后: 我们来看用一个代理实现将我们对data的数据访问绑定在vue对象上

设计模式

/**

solid 原则
s 单一职责
o 开放封闭
l 李氏置换
i 接口独立
d 依赖导致
*/
// 中介者模式

// 备忘录模式

// 命令模式

// 职责链模式

// 模板方法模式

// 策略模式 （减少 if … else …）
// class OrdinaryUser {
// buy () {
// console.log(‘普通用户购买’)
// }
// }
// class MemberUser {
// buy () {
// console.log(‘会员用户购买’)
// }
// }
// class VIPUser {
// buy () {
// console.log(‘VIP用户购买’)
// }
// }
// let u1 = new OrdinaryUser()
// let u2 = new MemberUser()
// let u3 = new VIPUser()
// u1.buy()
// u2.buy()
// u3.buy()

//享元模式 （共享数据 更小的内存）

// 组合模式 （整体 - 部分 的树形关系 Vnode）

// 桥接模式 （分开完成 + 然后合成 ）

// 原型模式 => prototype 复制自己 object.create()

// 状态模式 ( 减少 if … else … 有限状态机 javascript-state-machine)
// import StateMachine from ‘javascript-state-machine’
// let fsm = new StateMachine({
// init: ‘收藏’,
// transitions: [
// {
// name: ‘doStore’,
// from: ‘收藏’,
// to: ‘取消收藏’
// },
// {
// name: ‘deleteStore’,
// from: ‘取消收藏’,
// to: ‘收藏’
// }
// ],
// methods: {
// onDoStore: function () {
// console.log(‘收藏成功’)
// updateText()
// },
// onDeleteStore: function () {
// console.log(‘取消成功’)
// updateText()
// }
// }
// })

// let btn1 = document.getElementById(‘btn1’)
// btn1.addEventListener(‘click’, function () {
// if (fsm.is(‘收藏’)) {
// fsm.doStore()
// } else {
// fsm.deleteStore()
// }
// })
// function updateText () {
// // btn1.html(fsm.state)
// }

// class State {
// constructor (color) {
// this.color = color
// }
// handle (context) {
// console.log(turn to ${this.color})
// console.log(this)
// context.setState(this)
// }
// }
// class Context {
// constructor () {
// this.state = null
// }
// getState () {
// return this.state
// }
// setState (state) {
// this.state = state
// }
// }
// // 测试
// let context = new Context()

// let green = new State(‘green’)
// let yellow = new State(‘yellow’)
// let red = new State(‘red’)

// // 绿灯亮了
// green.handle(context)
// console.log(context.getState())
// // 黄灯亮了
// yellow.handle(context)
// console.log(context.getState())
// // 红灯亮了
// red.handle(context)
// console.log(context.getState())

// 迭代器 模式 （顺序访问一个集合） （有序数据集合：Array Map Set String TypedArray arguments NodeList）
// ( 用 for of 遍历对象等 就是 在用Iterator迭代器)
// class Iterator {
// constructor (container) {
// this.list = container.list
// this.index = 0
// }
// next () {
// if (this.hasNext()) {
// return this.list[this.index++]
// }
// return null
// }
// hasNext () {
// if (this.index >= this.list.length) {
// return false
// }
// return true
// }
// }

// class Container {
// constructor (list) {
// this.list = list
// }
// getIterator () {
// return new Iterator(this)
// }
// }

// let container = new Container([1, 2, 3, 4, 5])
// let iterator = container.getIterator()
// while (iterator.hasNext()) {
// console.log(iterator.next())
// }

// 观察者模式 （发布/订阅 一对N）
// class Subject {
// constructor () {
// this.state = 0
// this.observers = []
// }
// getState () {
// return this.state
// }
// setState (state) {
// this.state = state
// this.notifyAllObservers()
// }
// notifyAllObservers () {
// this.observers.forEach(observer => {
// observer.update()
// })
// }
// attach (observer) {
// this.observers.push(observer)
// }
// }

// class observer {
// constructor (name, subject) {
// this.name = name
// this.subject = subject
// this.subject.attach(this)
// }
// update () {
// console.log(${this.name} update, state: ${this.subject.getState()})
// }
// }
// // 测试代码
// let s = new Subject()
// let o1 = new observer(‘o1’, s)
// let o2 = new observer(‘o2’, s)
// let o3 = new observer(‘o3’, s)

// s.setState(1)
// s.setState(2)

// 外观模式
// 对外对内 只有一个接口 Facade
// 定义
// function bindEvent(elem, type, selector, fn) {
// if (fn == null) {
// fn = selector
// selector = null
// }
// }
// // 调用
// bindEvent(elem, ‘click’, ‘#div1’, fn)
// bindEvent(elem, ‘click’, fn)

// 代理模式
// 明星
// let star = {
// name: ‘张XX’,
// age: 25,
// phone: ‘13910733521’
// }
// // 经纪人
// let agent = new Proxy(star, {
// get: function (target, key) {
// if (key === ‘phone’) {
// // 返回经纪人自己的手机号
// return ‘18611112222’
// }
// if (key === ‘price’) {
// // 明星不报价，经纪人报价
// return 120000
// }
// return target[key]
// },
// set: function (target, key, val) {
// if (key === ‘customPrice’) {
// if (val < 100000) {
// // 最低 10w
// throw new Error(‘价格太低’)
// } else {
// target[key] = val
// return true
// }
// }
// }
// })

// // 主办方
// console.log(agent.name)
// console.log(agent.age)
// console.log(agent.phone)
// console.log(agent.price)

// // 想自己提供报价（砍价，或者高价争抢）
// agent.customPrice = 150000
// // agent.customPrice = 90000 // 报错：价格太低
// console.log(‘customPrice’, agent.customPrice)

// 装饰器模式
// import { readonly } from ‘core-decorators’
// class Person {
// @readonly
// name () {
// return ‘huhuuhu’
// }
// }

// let p = new Person()
// console.log(p.name())

// function log(target, name, descriptor) {
// console.log(descriptor)
// let oldValue = descriptor.value
// descriptor.value = function () {
// console.log(calling ${name} with, arguments)
// return oldValue.apply(this, arguments)
// }
// return descriptor
// }

// class Math {
// @log
// add(a, b) {
// return a + b
// }
// }

// let math = new Math()
// console.log(math.add(2, 4))

// function readonly (target, name, descriptor) {
// descriptor.writable = false
// return descriptor
// }

// class Person {
// constructor () {
// this.first = ‘A’
// this.second = ‘B’
// }

// @readonly
// name() {
// return this.first + this.second
// }
// }
// let p = new Person()
// console.log(p.name())
// p.name = function () {
// console.log(111)
// }

// function mixins(…list) {
// return function (target) {
// Object.assign(target.prototype, …list)
// }
// }

// const Foo = {
// foo() {
// console.log(‘foo’)
// }
// }

// @mixins(Foo)
// class MyClass {

// }

// let obj = new MyClass()
// obj.foo()

// function testDec(isDec) {
// return function (target) {
// target.isDec = isDec
// }
// }

// @testDec(false)
// class Demo {}

// console.log(Demo.isDec)

// @testDec
// class Demo {

// }

// function testDec(target) {
// target.isDec = true
// }
// console.log(Demo.isDec)

//适配器模式
// class Adaptee {
// specificRequest () {
// return ‘德国标准接头’
// }
// }

// class Target {
// constructor () {
// this.adaptee = new Adaptee()
// }
// request () {
// let info = this.adaptee.specificRequest()
// return ${info} - 转换器 - 中国标准接头
// }
// }

// let target = new Target()
// console.log(target.request())

// 单例模式
// class SingleObject {
// login () {
// console.log(‘login…’)
// }
// }

// SingleObject.getInstance = (function() {
// let instance
// return function () {
// if (!instance) {
// instance = new SingleObject()
// }
// return instance
// }
// })()

// let obj1 = SingleObject.getInstance()
// obj1.login()
// let obj2 = SingleObject.getInstance()
// obj2.login()
// console.log(obj1 === obj2)

// 工厂函数
// class Product {
// constructor (name) {
// this.name = name
// }
// init () {
// console.log(this.name)
// }
// }

// class Creator {
// create (name) {
// return new Product(name)
// }
// }

// // 测试
// let creator = new Creator()
// let p = creator.create(‘pppp’)
// p.init()

// class Car {
// constructor (num) {
// this.num = num
// }
// }

// class Camera {
// shot (car) {
// return {
// num: car.num,
// inTime: Date.now()
// }
// }
// }

// class Screen {
// show (car, inTime) {
// console.log(‘车牌号：’, car.num)
// console.log(‘停车时间：’, Date.now() - inTime)
// }
// }

// class Place {
// constructor () {
// this.empty = true
// }
// in () {
// this.empty = false
// }
// out () {
// this.empty = true
// }
// }

// class Floor {
// constructor (index, places) {
// this.index = index
// this.places = places || []
// }
// emptyPlaceNum() {
// let num = 0
// this.places.forEach(p => {
// if (p.empty) {
// num ++
// }
// })
// return num
// }
// }

// class Park {
// constructor (floors) {
// this.floors = floors || []
// this.camera = new Camera()
// this.screen = new Screen()
// this.carList = {}
// }
// in (car) {
// const info = this.camera.shot(car)
// const flour_index = parseInt(Math.random() 3 % 3)
// const place_index = parseInt(Math.random() 100 % 100)
// const place = this.floors[flour_index].places[place_index]
// place.in()
// info.place = place
// this.carList[car.num] = info
// }
// out (car) {
// const info = this.carList[car.num]
// const place = info.place
// place.out()
// this.screen.show(car, info.inTime)
// delete this.carList[car.num]
// }
// emptyNum () {
// return this.floors.map(floor => {
// return ${floor.index} 层 ，还有 ${floor.emptyPlaceNum()}个车位
// }).join(‘\n’)
// }
// }

// // 测试代码——————————
// // 初始化停车场
// const floors = []
// for (let i = 0; i < 3; i++) {
// const places = []
// for (let j = 0; j < 100; j++) {
// places[j] = new Place()
// }
// floors[i] = new Floor(i + 1, places)
// }
// const park = new Park(floors)

// // 初始化车辆
// const car1 = new Car(‘A1’)
// const car2 = new Car(‘A2’)
// const car3 = new Car(‘A3’)

// console.log(‘第一辆车进入’)
// console.log(park.emptyNum())
// park.in(car1)
// console.log(‘第二辆车进入’)
// console.log(park.emptyNum())
// park.in(car2)
// console.log(‘第一辆车离开’)
// park.out(car1)
// console.log(‘第二辆车离开’)
// park.out(car2)

// console.log(‘第三辆车进入’)
// console.log(park.emptyNum())
// park.in(car3)
// console.log(‘第三辆车离开’)
// park.out(car3)

// class Car {
// constructor(card, name, long) {
// this.card = card
// this.name = name
// this.long = long
// }
// showCard () {
// return this.card
// }
// showName () {
// return this.name
// }
// }

// class Fast extends Car {
// constructor(card, name, long) {
// super(card, name, long)
// this.money = 1
// }
// showPrice () {
// return this.money * this.long
// }
// }

// class Focus extends Car {
// constructor(card, name, long) {
// super(card, name, long)
// this.money = 2
// }
// showPrice () {
// return this.money * this.long
// }
// }

// let fast1 = new Fast(‘沪A11809’, ‘zhang111’, 5)
// console.log(fast1.showCard() + ‘-‘ + fast1.showName())
// console.log(fast1.showPrice())

// let focus1 = new Focus(‘川A88888’, ‘wang111’, 5)
// console.log(focus1.showCard() + ‘-‘ + focus1.showName())
// console.log(focus1.showPrice())

// class Person {
// constructor(name){
// this.name = name
// }
// getName() {
// return this.name
// }
// }

// let p = new Person(‘hy’)

// console.log(p.getName())

继承方式（原型链、组合模式、寄生组合式继承）
web性能优化、图片优化（雪碧图懒加载）
web安全：xss csrf sql注入
linux部分知识tail top
自定义dialog组件（注意:要用闭包封装模块）
nodejs http获取百度页面，把百度改为千百度
输入url过程
单纯的聊天(不记入面试)：看一个页面布局，说出布局想法
某个取值范围的随机数生成
nodejs优点
ajax请求过程
项目相关
谈人生规划
写一个继承，解释原型链
css规范化
闭包应用、模块
mvvm相关
知道哪些设计模式
两列布局
跨域方法
flex布局属性
事件流的三个阶段，哪些事件不能冒泡
webpack原理
闭包自由发挥
react优势(组件化、虚拟dom)
怎么设计好的组件
项目相关
反转链表
git命令了解哪些
github开源做过哪些，贡献过什么，pr过吗

实现Bind函数
if(!Function.prototype.bind){
Function.prototype.bind = function(context){
// 首先判断this是不是function
if(typeof this !== ‘function’){
// 抛出错误
}
var _this = this,
fNOP = function(){}, // 用于后面维护原型关系
args = Array.prototype.slice.call(arguments, 1), // 取出bind收到的参数，除去第一个作为上下文的参数并保存
fBound = function(){
_this.call(context, args.concat(Array.prototype.slice.call(arguments))) // 将bind接收的的参数和实际接收到的参数相连接
};
// 维护原型关系
if(this.prototype){
fNOP.prototype = this.prototype;
fBound.prototype = new fNOP;
}
return fBound;
}
}

实现节流函数
var throttle = function(fn){
var timer = null;
return function(){
var args = arguments, context = this;
clearTimeout(timer);
timer = setTimeout(function(){
fn.call(context, args)
}, 100)
}
}

这跟javascript高级程序设计中的实现方式差不多，其实严格来说这种实现方式并不叫节流函数，而是防抖函数：短时间内多次调用同一函数，只执行最后一次调用。
下面是改进：
var throttle = function(fn){
var timer = null;
return function(){
var args = arguments, context = this;
if(!timer){
timer = setTimeout(function(){
timer = null;
fn.call(context, args)
}, 100);
}
}
}

如此一来这个功能基本上就可以实现了，但是仍有一个问题，第一次调用的时候函数并没有执行，而是直接从第三次调用开始执行的，那么我们再改进一下，加一个第一次调用时的哨兵即可：
var throttle = function(fn){
var timer = null, first = true;
return function(){
var args = arguments, context = this;
if(first){
first = false;
fn.call(context, args)
}
if(!timer){
timer = setTimeout(function(){
timer = null;
fn.call(context, args)
}, 100);
}
}
}

实现一个函数，给三个参数，data是整形数组，m和sum都是一个整数，从data中取出n个整数，使它们的和为sum，求出一种组合即可。
我的思路是穷举data中的n个key的组合，假设data有6(n)个元素，从中取出3(m)个数，那么它key的组合就有：[0,1,2]、[0,1,3]、[0,1,4]、[0,1,5]、[0,2,3]、[0,2,4]、[0,2,5]、…、[3,4,5]
列出它的所有组合就好办了，直接用这些key去data里面取数，如果找到答案就退出程序。
下面仅给出穷举组合的算法，为了简单，相关参数写死并忽略全局污染：
// 数组a用于存放key的组合，data并没有出现，只给出data元素个数n
var a=[0,1,2], oldA = [],n = 6, len = a.length;
function listArr(key){
let same = true
for(let i = 0 ; i < len ; i++){
if(a[i] !== oldA[i]){
same = false
break
}
}
if(!same){
console.log(a)
oldA = Array.prototype.slice.call(a)
}
}
function list(currentKey, initValue){
if(currentKey >= len || initValue >= n){
return
}
for(let v = initValue ; v <= n + currentKey - len ; v++){
a[currentKey] = v
arguments.callee(currentKey+1, v+1)
listArr(currentKey);
}
}
list(0,0)

利用对象的属性不能相同的特点进行去重
Array.prototype.distinct = function (){
var arr = this,
i,
obj = {},
result = [],
len = arr.length;
for(i = 0; i< arr.length; i++){
if(!obj[arr[i]]){ //如果能查找到，证明数组元素重复了
obj[arr[i]] = 1;
result.push(arr[i]);
}
}
return result;
};
var a = [1,2,3,4,5,6,5,3,2,4,56,4,1,2,1,1,1,1,1,1,];
var b = a.distinct();
console.log(b.toString()); //1,2,3,4,5,6,56

用排序算法做，在排序比较的时候去除重复的元素…最快可以是nlogn

大数相加
//加法
function jia(a, b) {
//把a,b放进zong数组
var zong = [String(a), String(b)];
//创建fen数组
var fen = [];
//把a,b较大的放在前面
zong = getMax(zong[0], zong[1]);
//把zong数组里面的元素分成单个数字
zong[0] = zong[0].split(‘’);
zong[1] = zong[1].split(‘’);
//创建加0变量
var jialing;
//判断两个参数是否相同长度
if(!(zong[0].length == zong[1].length)) {
//创建0
jialing = new Array(zong[0].length-zong[1].length+1).join(‘0’);
//把0放进zong[1]前面
zong[1] = jialing.split(‘’).concat(zong[1]);
}
//创建补充上一位的数字
var next = 0;
//从个位数起对应单个计算
for(var i=(zong[0].length-1); i>=0; i–) {
//求和
var he = Number(zong[0][i]) + Number(zong[1][i]) + next;
//把求和的个位数先放进数组
fen.unshift(he%10);
//把求和的十位数放进补充上一位的数字，留在下一次循环使用
next = Math.floor(he/10);
//判断最后一次如果求和的结果为两位数则把求和的十位数加在最前面
if(i == 0 && !(next==0)) {
fen.unshift(next);
}
}
//把最后的结果转化成字符串
var result = fen.join(‘’);
//返回字符串
return result;
}

tostring和valueof有什么区别 [0]==0 true
valueof是返回最适合该对象类型的原始值，而tostring则是返回对象的字符串表示

position :relative,static,absolute,sticky,fixed,initial,inherit,unset
unset是initial和inherit的组合值…哪个属性有值就是哪个

GC 是标记清除法来回收内存 什么情况下添加标记，什么时候去除标记
当堆中的有效内存空间（available memory）被耗尽的时候，就会停止整个程序（也被成为stop the world），然后进行两项工作，第一项则是标记，第二项则是清除
标记：标记的过程其实就是，遍历所有的GC Roots，然后将所有GC Roots可达的对象标记为存活的对象。
清除：清除的过程将遍历堆中所有的对象，将没有标记的对象全部清除掉。
这种算法的缺点是:效率比较低（递归与全堆对象遍历），而且在进行GC的时候，需要停止应用程序，这会导致用户体验非常差劲,第二点主要的缺点，则是这种方式清理出来的空闲内存是不连续的

谈谈HTTPS 默认端口号为443

谈谈CDN加速 怎么选择？有什么算法？
CDN=更智能的镜像+缓存+流量导流 因而，CDN可以明显提高Internet网络中信息流动的效率
当用户访问已经加入CDN服务的网站时，首先通过 DNS重定向技术 确定最接近用户的最佳CDN节点，同时将用户的请求指向该节点
当用户的请求到达指定节点时，CDN的服务器（节点上的高速缓存）负责将用户请求的内容提供给用户。
　　具体流程为: 用户在自己的浏览器中输入要访问的网站的域名，浏览器向本地DNS请求对该域名的解析，本地DNS将请求发到网站的主DNS，主DNS根据一系列的策略确定当时最适当的CDN节点，并将解析的结果（IP地址）发给用户，用户向给定的CDN节点请求相应网站的内容。

写出下面会输出的值
if([]==false){console.log(1)};
if({}==false){console.log(2)};
if([]){console.log(3)}
if([1]==[1]){console.log(4)}
// 只输出1,3
false == false
true
[1]==[1]
false
[]==false
true
({})==false

写出节流函数,并说明在什么场景下使用
//其实是防抖
var debounce = function(delay, fn){
var last
return function(){
var ctx = this, args = arguments
clearTimeout(last)
last = setTimeout(function(){
fn.apply(ctx, args)
}, delay)
}
}

计算机网络状态码
旋转的硬币
同步发送几个url按顺序打开页面（promise+fetch+ajax）
正则表达式的运用，完成模板函数
new完成了什么事，用代码表达

上来就是两道算法…如果剑指Offer做过基本问题不大

算法题：

二叉树层序遍历(面试官提醒)

JS的全排列(10分钟)

HTTP支持的方法

GET和POST的区别

301和302的区别

如何避免301跳转https(在response中header)

TCP建立连接的三次握手过程

操作系统进程和线程的区别

线程的那些资源共享，那些资源不共享

设计模式：

单例，工厂，发布订阅

发布订阅怎么做

linux指令用的多吗，怎么进行进程间通信

kill指令了解过吗

如何画一个三角形(阿里一面同款)

CSS3中对溢出的处理(两小时前腾讯一面同款)

CSS选择器有哪些，优先级呢

ES6中用过哪些

promise的状态有那些

来讲讲JS的闭包吧

你有用到Express,讲讲Express(说对Koa2了解得多一些…)

那你用Koa2的话，讲讲两个的区别吧

能来讲讲JS的语言特性吗

最近在学啥

项目用到Java，反射来讲讲

Servlet呢？(基本忘完了…)

你用过什么数据库，来讲一下

MySQL里面的索引用过吗

B+树了解过吗

mongoDB有哪些特点讲讲

这个时候面试已经一个多小时了，面试官说，等五分钟看看，没问题就二面

二面

实现一个两列等高布局，讲讲思路

清除浮动的方法，能讲讲吗

怎么样让一个元素消失，讲讲

重排和重绘，讲讲看

HTTP状态码说说你知道的

讲讲304(我能介绍一下浏览器缓存机制吗)

那你讲讲看

强缓存、协商缓存什么时候用哪个

如何判断一个数组(讲到typeof差点掉坑里)

你说到typeof，能不能加一个限制条件达到判断条件(typeof只能判断是object,可以判断一下是否拥有数组的方法)

JS实现倒计时说说

为什么会不准

来来实现一下你的校正方法(此处编程10分钟)

JS实现跨域，方法讲讲

JSONP的缺点

跟面试官讲了一遍我了解的跨域方法，从前往后

React的特性讲讲

单项数据流了解过吗，说说

node的事件方法讲讲看

node的特性，适合处理什么场景

IO多路复用(没了解过…)

前端优化

从后端往前端讲，能讲很久

实现一个Ajax(写代码，忘记兼容IE的写法了…)

面试官：面完了，稍微等等，我去和HR商量一下

三面

我还以为没有第三面，结果视频请求就来了…

如果有一个很大的列表，像头条的新闻列表，用户看得多了，列表会越来越大，怎么处理，思考一下

(先开始说加载方面的优化…)

加载优化可以，那内存呢(替换啊分块存储啊，能想到的就说…)

如果有这样一个业务场景，一个模块A作为输入，BCD…等扩展模块可以在A做更改后展示A的原来内容或者加上CSS后的内容，想想思路

不用从DOM层面讲，我想听听广播方法和数据流控制

可以不用类Vue Object的原生方法实现这个双向数据绑定吗

(我是按照发布订阅来实现的)

恩这个满足了可扩展，那么我想改改问题…

你这个方法锁定了A作为输入源，如果A也可以作为输出模块呢，就是说再来了一个V模块，他做输入，ABC…模块变化输出，你增么扩展这个功能

冥思苦想…

在trigger函数触发的时候，设置一个target，调用每一个扩展模块的callback的时候，传递target给输出模块，统一管理