浏览web文章的时候 收集的大厂的一些面试题 当然是查漏补缺,学习一波了

px2rem ?
hotcss.js 会自动根据手机型号计算 dpr 的值，同时在 html 根标签内植入一个相应的 font-size 的值, 配合js根据设备的dpr设置html的font-size=“XX”来实现等比缩放

1
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
1
2
3
4
loaders: {
   css: 'vue-style-loader!css-loader!px2rem-loader?remUnit=40&remPrecision=8',
   scss: 'vue-style-loader!css-loader!px2rem-loader?remUnit=40&remPrecision=8!sass-loader'
}
js类继承的方法有哪些?
原型链法
Person.prototype = Animal.prototype; // 人继承自动物
Person.prototype.constructor = ‘Person’; // 更新构造函数为人
属性复制法
for(prop in Animal.prototype) {
Person.prototype[prop] = Animal.prototype[prop]
} // 复制动物的所有属性到人量边
Person.prototype.constructor = ‘Person’ // 更新构造函数为人
构造器应用法
function Person() {
Animal.call(this); // apply, call, bind方法都可以．细微区别，后面会提到．
}
对象类的继承
new Animal()

caller, callee和arguments分别是什么?
caller,callee之间的关系就像是employer和employee之间的关系，就是调用与被调用的关系，二者返回的都是函数对象引用．arguments是函数的所有参数列表，它是一个类数组的变量

1
2
3
4
5
6
7
8
function parent(param1, param2, param3) {
    child(param1, param2, param3);
}
function child() {
    console.log(arguments); // { '0': 'mqin1', '1': 'mqin2', '2': 'mqin3' }
    console.log(arguments.callee); // [Function: child]
    console.log(child.caller); // [Function: parent]
}
node有哪些核心模块?
EventEmitter, Stream, FS, Net和全局对象

node中的Buffer如何应用?
Buffer是用来处理二进制数据的，比如图片，mp3,数据库文件等.Buffer支持各种编码解码，二进制字符串互转

fs.createReadStream/fs.createWriteStream fs.readFileSync/fs.writeFileSync fs.readFile/fs.writeFile

node 中 async都有哪些常用方法，分别是怎么用?
它的目的是解决js中异常流程难以控制的问题
async.parallel并行执行完多个函数后，调用结束函数
async.series串行执行完多个函数后，调用结束函数
async.waterfall依次执行多个函数，后一个函数以前面函数的结果作为输入参数
async.map异步执行多个数组，返回结果数组
async.filter异步过滤多个数组，返回结果数组

node服务安全性的DDOS攻击?
最基本的DDOS攻击就是利用合理的服务请求来占用过多的服务资源，从而使合法用户无法得到服务的响应

Html5有哪些比较实用新功能?
File API支持本地文件操作 Canvans/SVG支持绘图 拖拽功能支持 本地存储支持 表单多属性验证支持 原生音频视频支持

描述new一个对象的过程?
创建一个新对象
this指向这个新对象
执行代码，即对this进行赋值
返回this

h5c3 的新特性?
h5
语义化标签 header、footer、aside、section、article、nav
表单输入类型 email、url、number、range、Date Pickers、search、color
表单属性 autocomplete、placeholder、form
视频音频 video、audio
画布/伸缩矢量图 canvas/svg
web存储 sessionStorage 和 localStorage
文件通讯协议 websocket

c3
盒模型 box-sizing
背景 background-image、background-size、background-origin
渐变 linear-gradient、radial-gradient
边框 border-radius、border-image
阴影 box-shadow、text-shadow
transform：translate rotate scale skew
过渡 transition
动画 keyframes、animation
弹性盒子 flex
媒体查询 @media

websocket?
WebSocket协议是基于TCP的一种新的网络协议。它实现了浏览器与服务器全双工(full-duplex)通信——允许服务器主动发送信息给客户端
http/tcp/ip => 应用层/网络层/传输层

websocket 和http区别?
1,websocket是持久连接的协议,而http是非持久连接的协议.
2,websocket是双向通信协议,模拟socket协议,可以双向发送消息,而http是单向的.
3,websocket的服务端可以主动向客服端发送信息,而http的服务端只有在客户端发起请求时才能发送数据,无法主动向客户端发送信息

HTTPS和HTTP的区别主要为以下四点：
1、https协议需要到ca申请证书，一般免费证书很少，需要交费。
2、http是超文本传输协议，信息是明文传输，https 则是具有安全性的ssl加密传输协议。
3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全

OPTIONS (预检请求) 请求方法的主要用途有两个?
向服务器询问是否允许跨域, 以及是否需要携带cookie等认证信息
当请求满足下述任一条件时，即应首先发送预检请求：
使用了下面任一 HTTP 方法： PUT DELETE
人为设置了对 CORS 安全的首部字段集合之外的其他首部字段: Accept Accept-Language Content-Language
Content-Type 的值不属于下列之一:application/x-www-form-urlencoded multipart/form-datatext/plain

在页面加载的时候，有两种情况：
一种情况是加载完DOM节点，但是图片、动画等文件没有加载，这时候可以触发ready方法;
另一种情况是页面所有内容都加载完成，包括DOM和图片、动画等，这时候可以触发onload方法。
而在页面关闭的时候，可以触发unload方法。

es6 array 的新方法:
Array.form Array.of Array.fill Array.copywith Array.find Array.findIndex Array.includes