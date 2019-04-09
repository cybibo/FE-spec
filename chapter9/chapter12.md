## 12.深度解析 new 原理及模拟实现
## 定义
> **new 运算符**创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。 ——（来自于MDN）

举个栗子

```js
function Car(color) {
    this.color = color;
}
Car.prototype.start = function() {
    console.log(this.color + " car start");
}

var car = new Car("black");
car.color; // 访问构造函数里的属性
// black

car.start(); // 访问原型里的属性
// black car start
```

可以看出 `new` 创建的实例有以下 2 个特性

* 1、访问到构造函数里的属性
* 2、访问到原型里的属性

## 注意点
ES6新增 `symbol` 类型，不可以使用 `new Symbol()`，因为 `symbol` 是基本数据类型，每个从`Symbol()`返回的 `symbol` 值都是唯一的。

```js
Number("123"); // 123
String(123); // "123"
Boolean(123); // true
Symbol(123); // Symbol(123)

new Number("123"); // Number {123}
new String(123); // String {"123"}
new Boolean(true); // Boolean {true}
new Symbol(123); // Symbol is not a constructor
```

## 模拟实现
当代码 `new Foo(...)` 执行时，会发生以下事情：

1. 一个继承自 `Foo.prototype` 的新对象被创建。
2. 使用指定的参数调用构造函数 `Foo` ，并将 `this` 绑定到新创建的对象。`new Foo` 等同于 `new Foo()`，也就是没有指定参数列表，`Foo` 不带任何参数调用的情况。
3. 由构造函数返回的对象就是 `new` 表达式的结果。如果构造函数没有显式返回一个对象，则使用步骤1创建的对象。

### 模拟实现第一步
`new` 是关键词，不可以直接覆盖。这里使用 `create` 来模拟实现 `new` 的效果。

`new` 返回一个新对象，通过 `obj.__proto__ = Con.prototype` 继承构造函数的原型，同时通过 `Con.apply(obj, arguments)`调用父构造函数实现继承，获取构造函数上的属性（【进阶3-3期】）。

实现代码如下

```js
// 第一版
function create() {
	// 创建一个空的对象
    var obj = new Object(),
	// 获得构造函数，arguments中去除第一个参数
    Con = [].shift.call(arguments);
	// 链接到原型，obj 可以访问到构造函数原型中的属性
    obj.__proto__ = Con.prototype;
	// 绑定 this 实现继承，obj 可以访问到构造函数中的属性
    Con.apply(obj, arguments);
	// 返回对象
    return obj;
};
```

测试一下

```js
// 测试用例
function Car(color) {
    this.color = color;
}
Car.prototype.start = function() {
    console.log(this.color + " car start");
}

var car = create(Car, "black");
car.color;
// black

car.start();
// black car start
```

完美！

不熟悉 `apply / call` 的点击查看：[【进阶3-3期】深度解析 call 和 apply 原理、使用场景及实现](https://juejin.im/post/5c060585e51d45480061b05f#heading-6)

不熟悉**继承**的点击查看：[JavaScript常用八种继承方案](https://juejin.im/post/5bcb2e295188255c55472db0)

### 模拟实现第二步
上面的代码已经实现了 80%，现在继续优化。

构造函数返回值有如下三种情况：

* 1、返回一个对象
* 2、没有 `return`，即返回 `undefined`
* 3、返回`undefined` 以外的基本类型

**情况1**：返回一个对象

```js
function Car(color, name) {
    this.color = color;
    return {
        name: name
    }
}

var car = new Car("black", "BMW");
car.color;
// undefined

car.name;
// "BMW"
```

实例 `car` 中只能访问到**返回对象中的属性**。

**情况2**：没有 `return`，即返回 `undefined`

```js
function Car(color, name) {
    this.color = color;
}

var car = new Car("black", "BMW");
car.color;
// black

car.name;
// undefined
```

实例 `car` 中只能访问到**构造函数中的属性**，和情况1完全相反。

**情况3**：返回`undefined` 以外的基本类型

```js
function Car(color, name) {
    this.color = color;
    return "new car";
}

var car = new Car("black", "BMW");
car.color;
// black

car.name;
// undefined
```

实例 `car` 中只能访问到**构造函数中的属性**，和情况1完全相反，结果相当于没有返回值。

**所以**需要判断下返回的值是不是一个对象，如果是对象则返回这个对象，不然返回新创建的 `obj`对象。

所以实现代码如下：

```js
// 第二版
function create() {
	// 创建一个空的对象
    var obj = new Object(),
	// 获得构造函数，arguments中去除第一个参数
    Con = [].shift.call(arguments);
	// 链接到原型，obj 可以访问到构造函数原型中的属性
    obj.__proto__ = Con.prototype;
	// 绑定 this 实现继承，obj 可以访问到构造函数中的属性
    var ret = Con.apply(obj, arguments);
	// 优先返回构造函数返回的对象
	return ret instanceof Object ? ret : obj;
};
```

## 【进阶3-4期】思考题解
问题：用 JS 实现一个无限累加的函数 `add`，示例如下：

```js
add(1); // 1
add(1)(2);  // 3
add(1)(2)(3)； // 6
add(1)(2)(3)(4)； // 10 

// 以此类推
```

实现：

```js
function add(a) {
	function sum(b) { // 使用闭包
    	a = a + b; // 累加
    	return sum;
 	}
 	sum.toString = function() { // 重写toString()方法
        return a;
    }
 	return sum; // 返回一个函数
}

add(1); // 1
add(1)(2);  // 3
add(1)(2)(3)； // 6
add(1)(2)(3)(4)； // 10 
```

我们知道打印函数时会自动调用 `toString()`方法，函数 `add(a)` 返回一个闭包 `sum(b)`，函数 `sum()` 中累加计算 `a = a + b`，只需要重写`sum.toString()`方法返回变量 `a` 就OK了。