## 6.深入浅出图解作用域链和闭包
`闭包是指有权访问另外一个函数作用域中的变量的函数`
关键在于下面两点：

* 是一个函数
* 能访问另外一个函数作用域中的变量

对于闭包有下面三个特性：

* 1、闭包可以访问当前函数以外的变量

```js
function getOuter(){
  var date = '815';
  function getDate(str){
    console.log(str + date);  //访问外部的date
  }
  return getDate('今天是：'); //"今天是：815"
}
getOuter();
```

* 2、即使外部函数已经返回，闭包仍能访问外部函数定义的变量

```js
function getOuter(){
  var date = '815';
  function getDate(str){
    console.log(str + date);  //访问外部的date
  }
  return getDate;     //外部函数返回
}
var today = getOuter();
today('今天是：');   //"今天是：815"
today('明天不是：');   //"明天不是：815"
```

* 3、闭包可以更新外部变量的值

```js
function updateCount(){
  var count = 0;
  function getCount(val){
    count = val;
    console.log(count);
  }
  return getCount;     //外部函数返回
}
var count = updateCount();
count(815); //815
count(816); //816
```

#### 作用域链
Javascript中有一个执行上下文(execution context)的概念，它定义了变量或函数有权访问的其它数据，决定了他们各自的行为。每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。

详情查看 [【进阶1-2期】JavaScript深入之执行上下文栈和变量对象](https://mp.weixin.qq.com/s?__biz=MzU3NjczNDk2MA==&mid=2247483746&idx=1&sn=06616d0bd52222cd0f2f038d77d913a3&chksm=fd0e12fdca799beb75b9eca2f2b1369d12381a5ec3231e17615a4f52463b8860c97cbf011d26&token=2048385331&lang=zh_CN#rd)

**作用域链**：当访问一个变量时，解释器会首先在当前作用域查找标示符，如果没有找到，就去父作用域找，直到找到该变量的标示符或者不在父作用域中，这就是作用域链。

作用域链和原型继承查找时的区别：如果去查找一个普通对象的属性，但是在当前对象和其原型中都找不到时，会返回undefined；但查找的属性在作用域链中不存在的话就会抛出**ReferenceError**。

作用域链的顶端是全局对象，在全局环境中定义的变量就会绑定到全局对象中。

#### 全局环境
##### 无嵌套的函数
```js
// my_script.js
"use strict";

var foo = 1;
var bar = 2;

function myFunc() {
  
  var a = 1;
  var b = 2;
  var foo = 3;
  console.log("inside myFunc");
  
}

console.log("outside");
myFunc();
```

**定义时**：当myFunc被定义的时候，myFunc的标识符（identifier）就被加到了全局对象中，这个标识符所引用的是一个函数对象（myFunc function object）。

内部属性[[scope]]指向当前的作用域对象，也就是函数的标识符被创建的时候，我们所能够直接访问的那个作用域对象（即全局对象）。

![](https://camo.githubusercontent.com/bf702f0df678f20694e04fb24fc2ea0e726434bc/687474703a2f2f646d697472796672616e6b2e636f6d2f5f6d656469612f61727469636c65732f6a735f636c6f737572655f322e706e67)

myFunc所引用的函数对象，其本身不仅仅含有函数的代码，并且还含有指向其被创建的时候的作用域对象。

**调用时**：当myFunc函数被调用的时候，一个新的作用域对象被创建了。新的作用域对象中包含myFunc函数所定义的本地变量，以及其参数（arguments）。这个新的作用域对象的父作用域对象就是在运行myFunc时能直接访问的那个作用域对象（即全局对象）。

![](https://camo.githubusercontent.com/74f0b304b1940aab6f4e96949a4708f088e1ed82/687474703a2f2f646d697472796672616e6b2e636f6d2f5f6d656469612f61727469636c65732f6a735f636c6f737572655f332e706e67)

##### 有嵌套的函数
当函数返回没有被引用的时候，就会被垃圾回收器回收。但是对于闭包，即使外部函数返回了，函数对象仍会引用它被**创建时**的作用域对象。

```js
"use strict";
function createCounter(initial) {
  var counter = initial;
  
  function increment(value) {
    counter += value;
  }
  
  function get() {
    return counter;
  }
  
  return {
    increment: increment,
    get: get
  };
}

var myCounter = createCounter(100);
console.log(myCounter.get());   // 返回 100

myCounter.increment(5);
console.log(myCounter.get());   // 返回 105
```

当调用 createCounter(100) 时，内嵌函数increment和get都有指向createCounter(100) scope的引用。**假设**createCounter(100)没有任何返回值，那么createCounter(100) scope不再被引用，于是就可以被垃圾回收。

![](https://camo.githubusercontent.com/faa2434436b9420b99ae45f42a5a55311250a7af/687474703a2f2f646d697472796672616e6b2e636f6d2f5f6d656469612f61727469636c65732f6a735f636c6f737572655f342e706e67)

但是createCounter(100)实际上是有返回值的，并且返回值被存储在了myCounter中，所以对象之间的引用关系如下图：

![](https://camo.githubusercontent.com/13a4e54f23639c5b274014f0caa21115760052f6/687474703a2f2f626c6f672e6c6561706f61686561642e636f6d2f323031352f30392f31352f6a732d636c6f737572652f6a735f636c6f737572655f352e706e67)

即使createCounter(100)已经返回，但是其作用域仍在，并且只能被内联函数访问。可以通过调用myCounter.increment() 或 myCounter.get()来直接访问createCounter(100)的作用域。

当myCounter.increment() 或 myCounter.get()被调用时，新的作用域对象会被创建，并且该作用域对象的父作用域对象会是当前可以直接访问的作用域对象。

调用`get()`时，当执行到`return counter`时，在get()所在的作用域并没有找到对应的标示符，就会沿着作用域链往上找，直到找到变量`counter`，然后返回该变量。

![](https://camo.githubusercontent.com/4cff42540fb6a6ebd532389538101a16af65e7cb/687474703a2f2f646d697472796672616e6b2e636f6d2f5f6d656469612f61727469636c65732f6a735f636c6f737572655f362e706e67)

单独调用increment(5)时，参数value保存在当前的作用域对象。当函数要访问counter时，没有找到，于是沿着作用域链向上查找，在createCounter(100)的作用域找到了对应的标示符，increment()就会修改counter的值。除此之外，没有其他方式来修改这个变量。闭包的强大也在于此，能够存贮私有数据。

![](https://camo.githubusercontent.com/5e61b55212aae4198b0bdcb56179baf850ea3bd4/687474703a2f2f646d697472796672616e6b2e636f6d2f5f6d656469612f61727469636c65732f6a735f636c6f737572655f365f696e632e706e67)

创建两个函数：`myCounter1`和`myCounter2`

```
//my_script.js
"use strict";
function createCounter(initial) {
  /* ... see the code from previous example ... */
}

//-- create counter objects
var myCounter1 = createCounter(100);
var myCounter2 = createCounter(200);
```
关系图如下
![](https://camo.githubusercontent.com/ae96f45e96a12f66b88d8bc2126d815c197759b7/687474703a2f2f646d697472796672616e6b2e636f6d2f5f6d656469612f61727469636c65732f6a735f636c6f737572655f372e706e67)

myCounter1.increment和myCounter2.increment的函数对象拥有着一样的代码以及一样的属性值（name，length等等），但是它们的[[scope]]指向的是不一样的作用域对象。