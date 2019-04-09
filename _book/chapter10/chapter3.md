 浏览web文章的时候 收集的大厂的一些面试题 当然是查漏补缺,学习一波了

原文问题链接: https://juejin.im/post/5bc92e9ce51d450e8e777136

—————- 今日头条-有赞-挖财 —————-

————————— 今日头条 —————————
对async、await的理解，内部原理?
async、await 是一种异步编程的解决方式
await/async和yield都被编译器在编译时转化为了状态机
因为(C# 和 .NET 都提供了这个功能)

介绍下Promise，内部实现?
是一种异步解决方案,简单点就是一个容器,状态机
内部实现 就是 pending => fullield rejected 用 then(f1, f2)

清除浮动?

1
2
3
4
5
6
7
::before, ::after {
  display: block;
  content: '',
  clear: both;
}

overflow:hidden
定位问题（绝对定位、相对定位等）?

从输入URL到页面加载全过程?

tcp3次握手?
tcp属于哪一层（1 物理层 -> 2 数据链路层 -> 3 网络层(ip)-> 4 传输层(tcp) -> 5 应用层(http)）?

redux的设计思想 ?
一个全局的状态管理中心, 不用一步一步的 props 传递数据, 使数据的使用更加方便

接入redux的过程? 绑定connect的过程? connect原理?
react-redux 提供的两个api Provider标签中 传入全局的store store里面有reducer 里面有对应的 action的处理方法 用户使用是,通过connect(mapStateToProps, mapDispatchToProps)(component) 传入到组件中
每一个action-types 都有对应的修改store 里的state的方法

webpack介绍?
Webpack 是一种 模块化的 打包方案
把所有的代码都模块化, 通过统一的入口文件(一般是app.js), 打包出统一的出口
根据 loader plugin 来个性化打包的 方法 插件 满足不同的项目需求

== 和 ===的区别，什么情况下用相等==?
== 会进行隐式转换
=== 得判断数据的类型是否相同
现在 基本上不用 ==, 不晓得什么情况下能用吧

bind、call、apply的区别?
都是指定函数的this指向, 来让函数实现自身没有的方法
bind 底层还是 call apply 的封装
call 参数是一个一个的, apply 参数是数组

动画的了解?

介绍下原型链（解决的是继承问题吗）?
继承: 原型链继承、借用构造函数继承(call, apply)、组合继承
原型链继承 只是继承的一种方式

对跨域的了解?

————————— 有赞 —————————
Linux 754 介绍?
最后一层目录权限为754, 直接进入到最后一层目录 chmod 754 5/4/3/2/1

介绍冒泡排序，选择排序，冒泡排序如何优化?
冒泡排序:
1.依次两两比较相邻的元素，如果第一个比第二个大，则进行交换
2.经过第一轮比较之后，最大的数已经出现在数组最后一个位置了
3.然后再对除了最后一个元素外的所有数都重复一遍上述比较，结束后第二个的数会到达数组倒数第二个位置
4.再依次对剩下的数进行重复，直到排序完毕

1
for (let j = 0; j < arr.length - i; j++) {}
优化: 在第一层循环内加个 bool = true 在第二个循环的 if 里如果交换 就设置 bool = false

选择排序:
1.从数组的开头起，将第一个元素和其他所有元素都进行一次比较，选择出最小的元素放在数组的第一个位置
2.然后再从第二个元素开始，将第二个元素和除第一个之外的所有元素进行一次比较，选择出最小的元素放在数组的第二个位置
3.对后面的第三，第四……的元素分别重复上面的步骤，直到所有的数据完成排序

transform动画和直接使用left、top改变位置有什么优缺点?
设置一次的初始末尾位置, 用浏览器的GPU去处理动画, 页面更加流畅, 减少页面的重绘和回流 性能更高

如何判断链表是否有环?
“快慢指针”: 用两个指针 fast 每次走两步 slow 每次走一步; 当 fast 再次与 slow 相遇的时候 就说明链表有环

介绍二叉搜索树的特点?
二叉树就是树的每个节点最多只能有两个子节点
二叉搜索树由于其独特的数据结构，使得其无论在增删，还是查找，时间复杂度都是O(h)，h为二叉树的高度。因此二叉树应该尽量的矮，即左右节点尽量平衡

介绍暂时性死区 ?
let const 变量申明前引用报错 只能申明一次变量 不会变量提升 按申明的位置开始起作用

ES6中的Map和原生的对象有什么区别?
Object的键只能是字符串，Map的键可以是任意类型的值（包括对象），所以Map是一种更完善的Hash结构实现

观察者和发布-订阅的区别?
观察者: 多个对一个的观察 并更新
发布-订阅: 一个通过调度中心对多个的 通知

react异步渲染的概念,介绍Time Slicing 和 Suspense?
一个组件不会阻塞其他组件的渲染, 但是会异步的
如果设备网速够快的话, 感觉上是同步的
异步的渲染页面,只展示最终的渲染状态, 就是等待页面的数据获取到了之后, 再一次性的把页面渲染出来, 如果是多个请求的数据,可以先渲染出来 (有点把后端服务端渲染的页面 放到前端来渲染的感觉, 中间请求数据的时候 给一个请求提示)

react生命周期的改变?

react中props改变后在哪个生命周期中处理?
componentWillReciveProps => shouldComponentUpdate

介绍纯函数?
就是 return 一个jsx 语法的模块 , 不 extends React.Component, 不用组件实例化, 也没有生命周期, 无state，只有props 纯渲染的, 效率更高

前端性能优化?

pureComponent 和 FunctionComponent 区别?
一个是为了提高性能的浅比较组件, 一个是可以纯函数组件或者高阶组件

介绍JSX?

1
2
3
// 利用HTML语法来创建虚拟DOM。
// 当遇到 < JSX就当HTML解析，遇到 { 就当JavaScript解析
// on 绑定事件, style={{}} 设置行内样式
如何做RN在安卓和IOS端的适配?

RN为什么能在原生中绘制成原生组件（bundle.js）?

介绍虚拟DOM?
含有标签的属性和方法的一个node节点对象

如何设计一个localStorage，保证数据的实效性?
是一种你不主动清除它，它会一直将存储数据存储在客户端的存储方式, 为了数据的时效性, 可以和数据一起存一个data时间戳, 也可以是token等
localStorage.setItem(‘date’,Json.stringIfy(date))

如何设计Promise.all()

介绍高阶组件?
把基础组件当做函数参数,传入到函数中, 并做相应的操作, 再渲染组件

sum(2, 3)实现sum(2)(3)的效果?
函数柯里化, 保证一次传入更少的参数, 做更加明确的事情

react性能优化?

两个对象如何比较是否相等?

function diff(obj1,obj2){
    let o1 = obj1 instanceof Object;
    let o2 = obj2 instanceof Object;
    /*  判断不是对象  */
    if(!o1 || !o2){
        return obj1 === obj2;
    }

    //Object.keys() 返回一个由对象的自身可枚举属性(key值)组成的数组,例如：数组返回下表：let arr = ["a", "b", "c"];console.log(Object.keys(arr))->0,1,2;
    if(Object.keys(obj1).length !== Object.keys(obj2).length){
        return false
    }

    for(var attr in obj1){
        var t1 = obj1[attr] instanceof Object;
        var t2 = obj2[attr] instanceof Object;
        if(t1 && t2){
            return diff(obj1[attr],obj2[attr]);
        }else if(obj1[attr] !== obj2[attr]){
            return false;
        }
    }
    return true;
}
————————— 挖财 —————————
JS的原型?
prototype

变量作用域链?
以前的 var 声明变量, 存在变量提升, 分为全局作用域和函数作用域, 按照作用域链从内到外,往上查找

call、apply、bind的区别?

防抖和节流的区别?

介绍各种异步方案?

react生命周期?

介绍Fiber?
React Fibre是React核心算法正在进行的重新实现 fiber代表一个工作单元
React Fiber的目标是提高其对动画，布局和手势等领域的适用性
它的主体特征是增量渲染：能够将渲染工作分割成块，并将其分散到多个帧中

前端性能优化?

介绍DOM树对比?
diff 算法 一层一层往下比较

react中的key的作用?
有循环的时候 确定某一个虚拟 DOM 节点, 可以帮助 diff 算法 更快的更新 虚拟DOM

如何设计状态树?
一个公共的reducer 然后就是各个页面级别的reducer

介绍css，csrf?
xss 跨站脚本攻击 客户端输入的脚本
csrf 跨站请求伪造 后台模仿的请求攻击

http缓存控制?
1.0 max-age 1.1 E-tag Last-Modified(If-Modified-Since) 看http那篇

项目中如何应用数据结构?
不是复杂的项目, 数据结构应该尽量简单
静态数组适合元素不超过100的场合
动态数组适合元素不超过1000的场合
链表适合元素不超过3000的场合

native提供了什么能力给RN?

如何做工程上的优化?

shouldComponentUpdate是为了解决什么问题?
解决了组件是否需要再次 render 渲染, 如果并不需要渲染, 可以提升渲染效率

如何解决props层级过深的问题?

用 redux 全局状态管理
顶层组件的 context this.context.属性 去引用

前端怎么做单元测试?
mocha chai should

webpack生命周期?
webpack打包的整个过程?
webpack将创建所有应用程序的依赖关系图表。图表的起点被称之为入口起点(entry point)
loader可以使你在require()或”加载”模块时预处理文件。因此，loader类似于其他构建工具中“任务(task)”，并提供了处理前端构建步骤的强大方法
由于loader仅在每个文件的基础上执行转换，而插件(plugins)最常用于（但不限于）在打包模块的“compilation”和“chunk”生命周期执行操作和自定义功能，包括打包优化压缩及配置编译时的变量等功能
将所有的资源(assets)归拢在一起后，还需要告诉webpack在哪里打包应用程序。webpack的output属性描述了如何处理归拢在一起的代码(bundled code)

1
output: { path: path.resolve(__dirname, 'dist'), filename: 'bundle.js'}
常用的plugins?

pm2怎么做进程管理，进程挂掉怎么处理 ?
PM2是一个带有负载均衡功能的Node.js应用的进程管理器。它允许你永远保持应用的存活,重新加载无需停机

不用pm2怎么做进程管理