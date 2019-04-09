浏览web文章的时候 收集的大厂的一些面试题 当然是查漏补缺,学习一波了

原文问题链接: https://www.nowcoder.com/discuss/137094?type=2&order=0&pos=101&page=1

—————- 美团 - 网易有道 - 58 - 滴滴 —————-

——————– 美团 ——————–

1、自我介绍、简历上项目和实习经历的描述
2、前端基础、网络、数据结构和算法基础：（凭记忆想问题了）
（1）闭包、对原型链的理解、prototype和proto的区别
（2）隐藏元素的方法
（3）js的语言特点，对比一下其他的语言
（4）http2.0和http1.x的区别
（5）cookie的理解延伸到CSRF（跨站请求伪造）攻击
（6）最基本的排序，描述了冒泡排序
（7）对前端工具的了解
…(总觉得还有什么我忘了的)
3、对个人的认识
为什么学前端、认为自己学前端有什么优势、怎么学前端（说到看书的时候让说一下比较印象深刻的点）

面试官评价（学习或者做项目的时候最好理解原理深入挖掘）

——————– 网易有道 ——————–

1、自我介绍、项目描述，遇到了什么问题
2、前端基础
（1）描述一下闭包，说一个简单的例子，闭包的原理
（2）清除浮动
（3）实现一个轮播图
（4）描述盒模型，宽度的计算、margin50%是相对于宽还是高
（5）事件委托
（6）箭头函数
（7）自己对ES6还有什么了解
（8）前端优化可以从哪些方面考虑
（9）状态码有没有了解
（10）数组去重
（11）找到数组中最大的数

——————– 58 ——————–

号码段为131到139的11位手机号码正则校验： ‘/^13[1-9][0-9]{8}$/‘
移动端，如何在html中通过链接调起拨打电话10086

以下代码的输出结果： “teacher” ; Uncaught ReferenceError: s is not defined
{
var t=‘teacher’;
let s=‘student’;
}
console.log(t);
console.log(s);

HTTP协议的状态码200、400、500分别代表什么？200: OK; 400: Bad Request; 500: Internal Server Error

JavaScript算术运算：‘10’+ 1结果为‘101’ ‘10’-1结果为96. var ting = 1, shi=3, wei=2;用ES6字符串模板的方式输出：1室3厅2卫。${ting}室${shi}厅${wei}卫

用ES6解构的方式，将下面代码中的obj.name赋值给n，obj.age赋值给a：let {name: n, age: a} = obj;
let obj = {name:’韩梅梅’, age:’20’};
let n, a;

HTTP协议默认的端口号80HTTPS协议的端口号443
名词解释：MVCmodel-view-controller、MVPmodel-view-presenter、MVVMmodel-view-viewmodel
Flex布局实现容器box内部元素item垂直居中对齐。.box {display: flex; align-items: center }

CSS3的box-sizing的取值及各值的说明
function switchCase(value){
switch(avlue){
case ‘0’:console.log(‘case 0’);
case ‘1’:console.log(‘case 1’);break;
case undefined:console.log(‘undefined’);break;
default:console.log(‘default’);
}
}

// 写出下列输出结果
switchCase(0);
switchCase(‘0’);
switchCase()

列举出通过CSS样式隐藏元素的方法，并说明其区别

请写出下面代码的执行结果：
var s = {
s: ‘student’,
getS: function(){
console.log(this.s);
}
};
var t = {
s: ‘teaher’
};

var getS = s.getS;
var getS1 = getS.bind(s);

// 写出以下输出结果
s.getS();
s.getS.apply(t);
getS();
getS1.call(t)

列出移动端开发中适配各种屏幕尺寸的解决方案(至少3种)

——————– 滴滴 ——————–

一面：
1、对vue怎么看（balabala）。
2、你的项目的对自己成长最大的部分在哪。
3、组件中的通信，着重问了兄弟组件的通信。
4、小程序的生命周期。
5、讲一下js的继承、作用域链。
6、px、rem、em的区别。
7、居中的样式如何实现。
8、树的遍历查询。（介绍一下思想，没手撕）

二面：
1、组件的封装。
2、手机端不同屏幕如何适配。
3、动画的优化，接着问为什么transition比margin的性能好。
4、垂直居中的样式怎么实现。（这里严重怀疑滴滴面试是不知道以前的面试问过什么的）
5、闭包。
6、vue的组件通信和vuex。（再次怀疑滴滴面试是不知道以前的面试问过什么的）

三面：
1、设计模式介绍一下。
2、什么是虚拟dom。
3、http缓存介绍一下。
4、前端发展的瓶颈在哪（自己附加了前端的性能优化）。
5、快排算法。