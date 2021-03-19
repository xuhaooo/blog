# 一、JS 里为什么会有 this
## 1. 问题的由来

```js
var obj = {
	fn: function() { console.log(this.x) },
	x: 1
}
var fn = obj.fn
var x = 2

obj.fn() // 1
fn() //2
```

obj.fn 和 fn 指向同一个函数，但是执行结果却不一样！

为什么会这样呢？原因在于函数内部的 this

this 指的是函数运行时所在的环境

对于 `obj.fn()` 来说，fn 运行在 obj 环境，this 指向 obj

对于 `fn()` 来说，fn 运行在全局环境，this 指向全局环境

但是为什么 `obj.fn()` 运行在 obj 环境，而 `var fn = obj.fn` 之后，`fn()` 就在全局环境执行呢？

## 2. 解释

### JS 内存角度

```js
var obj =  { f: 1 }
```

上述代码是将一个对象赋值给一个变量 obj

JS 引擎会在内存中生成这个个对象，然后将这个对象的内存地址赋值给 obj

但是问题在于对象的属性值有可能是一个函数

```js
var obj = { f: function(){} }
```

函数也是对象，JS 引擎会单独给它开辟一块内存，然后将其内存地址赋值给 f

由于函数是一个单独的值，所以它可以在不同环境执行

而函数可以运行在不同的环境中，同时 JS 中可以在函数内部，引用当前环境的其他变量

```js
var f = function(){
	console.log(x)
}
```

这个 x 是由函数运行的环境提供的

于是问题就来了：

既然函数可以在不同的环境里面运行，且我需要在函数内部拿到当前环境提供的 x

那我如何在函数体内部去获取到当前的运行环境呢？

于是，this 出现了，它出现的目的就是：在函数内，指代函数当前运行的环境

```js
var f = function(){
	console.log(this.x)
}
// this.x 指的就是函数 f 运行环境中的 x
```

### this 设计者角度

有一个对象：

```js
var user = {
	name: '张三',
	gender: '男',
	nationality: '中国',
	introduce: function() {}
}
```

有一个需求：调用 `user.introduce(..)`，打印出「你好，我是张三，性别男，我是中国人」

很傻的实现：

```js
var user = {
	name: '张三',
	gender: '男',
	nationality: '中国',
	introduce: function(name, gender, nationality) {
		console.log(`你好，我是${name}，性别${gender}，我是${nationality}人`)
	}
}

user.introduce(user.name, user.gender, user.nationality)
```

改进的实现：

```js
var user = {
	name: '张三',
	gender: '男',
	nationality: '中国',
	introduce: function(self) {
		console.log(`你好，我是${self.name}，性别${self.gender}，我是${self.nationality}人`)
	}
}

user.introduce(user)
```

稍微好一点，但是如果把调用里面的参数 self 省掉，直接使用 `user.introduce()` 就好了

可问题是，如果 `user.introduce()` 没有实参，那怎么接收到 user 呢？

方法一：依然把调用里的第一个参数 self 当做 user，这样形参就永远比实参多一个 self

方法二：隐藏 self，然后用 this 来访问 self

JS 的设计者选择了方法二，于是上面的需求实现变成

```js
var user = {
	name: '张三',
	gender: '男',
	nationality: '中国',
	introduce: function() {
		console.log(`你好，我是${this.name}，性别${this.gender}，我是${this.nationality}人`)
	}
}

user.introduce()
```

这个时候，很多人会很困惑，这个 this 是是个啥啊？

事实上，this 是隐藏的第一个形参，当你调用 `user.introduce()` 的时候，user 会变成 this

# 二、如何确定 this

## 1. 具体情况具体对待 ——— 术

1. 全局环境

    纯粹的函数调用，全局环境中使用 this。在严格模式下，this 是 undefined；非严格模式下，this 是 window/global

    ```js
    function fn() {
    	console.log(this === window)
    }
    fn() // true
    this === window // true
    ```

2. 构造函数

    构造函数中的 this，指的是实例对象

    ```js
    var Obj = function(xxx) {
    	this.a = xxx
    }
    var obj = new Obj('hello')
    console.log(obj.a) // "hello"
    ```

    注意，如果构造函数中返回一个对象，this 是这个返回的对象

    ```js
    var Obj = function() {
    	this.x = 'abc'
    	return {x: 'def'}
    }
    var obj = new Obj()
    console.log(obj.x) // "def"
    ```

3. 对象的方法

    如果对象的方法里面包含 this，this 就是方法运行时所在的对象

    ```js
    var x = 'abc'
    var obj = {
    	x: 'def',
    	fn: function(){
    		console.log(this.x)
    	}
    }
    obj.fn() // "def"
    ```

    但存在复杂的多层调用关系时，this指向最后调用函数的对象

4. 箭头函数

    箭头函数没有 this，不具备讨论 this 的条件

    也就是说，箭头函数里面的 this 就是外面的 this，即箭头函数定义时所在的对象

    可以看成外面的 this 直接穿透进来，所以并不存在箭头函数里的 this 的指向问题

    ```js
    this === window // true
    var fn = (){ console.log(this) } 
    fn() // window
    ```

## 2. 通过正确的调用形式 —— 道

1. 正确的调用形式

    先从函数调用讲起，ES5 里面有 3 种函数调用形式：

    ```js
    fn(p1, p2)
    obj.child.method(p1, p2)
    fn.call(context, p1, p2) // 先不说 apply
    ```

    虽然我们更常用的是前面两种形式，但是事实上，第三种方式才是正常的形式

    前面两种是 JS 设计者投喂的语法糖，可以等价的转换为第三种形式

    ```js
    fn.call(undefind, p1, p2)
    obj.child.method.call(obj.child, p1, p2)
    ```

    这样 this 就非常好确定了，this 就是 `fn.call(contex, p1, p2)` 里面的 context

    然后我们来看一下这样的代码

    ```js
    function fn(){
      console.log(this)
    }

    fn()
    fn.call(undefined) // 转换一下
    // window
    ```

    按理说打印出来的应该是 undefind，但是浏览器中有这样的规则：

    > 如果你传的 context 是 undefined/null，那么 window 对象就是默认的 context（严格模式下 undefined 是默认的 context）

    如果你不希望这里的 this 不是 window，`fn.call(obj)` 之后里面的 this 就是 obj 了

    再看这样的代码

    ```js
    function fn (){ console.log(this) }
    var arr = [fn, fn2]
    arr[0]()

    // 转换代码

    arr.0.call(arr) // 虽然语法是错的，但是我们可以想象成这样
    // arr
    ```

2. 被封装的 this

    上面所说的通过 .call 形式来调用函数，来确定 this 的值

    这种方式非常的简单易懂，但是有些 API 将 this 封装起来了，其里面的 this 依然让人困惑

    ```js
    // 原生 JS 事件处理函数中的 this
    button.addEventListener('click', function handler(){
    	console.log(this) // this 是什么？
    })

    // jQuery 事件处理函数中的 this
    $ul.on('click', 'li', function(){
    	console.log(this) // this 是什么？
    })
    ```

    这种时候你就需要去看源码或看文档了

    MDN 文档里这样描述：

    > 当使用 addEventListener() 为一个元素注册事件的时候，句柄里的 this 值是该元素的引用。其与传递给句柄的 event 参数的 currentTarget 属性的值一样。

    于是可以假想浏览器的源码是这样写的

    ```js
    handler.call(event.currentTarget, event)
    ```

    jQuery 文档里面这样描述：

    > 当jQuery的调用处理程序时，this关键字指向的是当前正在执行事件的元素。对于直接事件而言，this 代表绑定事件的元素。对于代理事件而言，this 则代表了与 selector 相匹配的元素。(注意，如果事件是从后代元素冒泡上来的话，那么 this 就有可能不等于 event.target。)若要使用 jQuery 的相关方法，可以根据当前元素创建一个 jQuery 对象，即使用 $(this)。

3. 总结

    如何确定 this 的值？

    - this 就是你 call 一个函数时，传入的第一个参数
    - 看源码中的对应函数是怎么被 call 的
    - 看文档解释
    - 利用好 console.log(this)
4. 实践一下：

    ```js
    var a = {
    	name: "里面的name",
    	sayName: function(){
    		console.log("this.name = " + this.name)
    	}
    }
    var name = "外面的name"
    function sayName(){
    	var sss = a.sayName
    	sss() // this.name = ?
    	a.sayName() // this.name = ?
    	(a.sayName)() // this.name = ?
    	(b = a.sayName)() // this.name = ?
    }
    sayName()

    // 转换代码

    var a = {
    	name: "里面的name"
    	sayName: function(){
    		console.log("this.name = " + this.name)
    	}
    }
    var name = "外面的name"
    function sayName(){
    	var sss = a.sayName
    	sss.call(undefined) // "外面的name"
    	a.sayName.call(a) // "里面的name"
    	(a.sayName).call(a) // "里面的name"
    	b.call(undefined) // "外面的name"
    }
    sayName()
    ```

    再来一份？

    ```js
    var length = 10
    function fn(){
    	console.log(this.length)
    }
    var obj = {
    	length: 5,
    	method: function(fn){
    		fn()
    		arguments[0]()
    	}
    }
    obj.method(fn, 1) // 输出是什么？

    // 改写代码

    var length = 10
    function fn(){
    	console.log(this.length)
    }
    var obj = {
    	length: 5,
    	method: function(fn){
    	fn.call(undefined)
    	arguments.0.call(arguments)
    		}
    }
    obj.method.call(obj, fn, 1)
    // 10 2
    ```
# 三、如何强制指定 this
## 1. call()
- 用于指定函数内部 this 的指向（即函数执行时所在的作用域），然后在所指定的作用域中，调用该函数

```js
var obj = {}
var fn = function() {
	return this
}

fn() === window // true
fn.call(obj) === obj // true
```

- 参数一，应该是一个对象，如果是空、null、undefined，默认传入全局对象；如果是原始值，则会被自动包装成一个对象
- 还可以接受多个参数，后续参数则是函数调用时所需的参数
- 回到文章开头的例子，这才是 JS 设计者提供的无糖调用函数的方法
- `user.introduce.call(user)`等价于`user.introduce()`，第一个参数是一个显式的 user，不含糖
- 这样 `user.introduce.call()`的参数可以是任何对象，也就是说 `user.introduce` 虽然是 user 的方法，但是可以调用在任何对象上
- 很多不喜欢 this 的人还可以通过 `obj.fn.call(unll, 1, 2, 3)` 来禁用 this

## 2. apply()
- 与 `call()` 的用法类似，唯一的区别就是，它接受一个数组作为函数执行时的参数

```js
function fn(a,b){
	console.log(a+b)
}
fn.call(null, 1, 2) // 3
fn.apply(null, [1, 2]) // 3
```

## 3. bind()
- 用于绑定函数调用的第一个参数即 this（或绑定多个参数）到某个对象上，然后返回一个新函数
- 注意，它返回的是绑定了一个对象的新函数

```js
var x = 1
var obj = {
	x: 2,
	fn1: function() { console.log(this.x) }
}

obj.fn1() // 等价于 obj.fn.call(fn)
// 2

var fn2 = obj.fn1
fn2() // 等价于 fn2.call(undefined)
// 1

var fn3 = fn2.bind(obj)
fn3() // 等价于 fn3.call(obj)
// 1
```

# 参考资料

- [JS 里为什么会有 this](https://zhuanlan.zhihu.com/p/30164164)
- [JS 的 this 原理](https://www.ruanyifeng.com/blog/2018/06/javascript-this.html)
- [this 关键字](https://wangdoc.com/javascript/oop/this.html)
- [this 的值到底是什么](https://zhuanlan.zhihu.com/p/23804247)
- [你怎么还没搞懂 this](https://zhuanlan.zhihu.com/p/25991271)