## 一、概述
1. 事件的本质是程序各个组成部分之间的一种通信方式，也是异步编程的一种实现。DOM 事件模型，就是通过监听函数对事件作出反应。事件发生后，浏览器监听到这个事件，就会执行对应的监听函数，这是事件驱动编程模式的主要编程方式。
2. JavaScript 并不支持事件，DOM 事件与 JavaScript 之间并没有从属关系，事件模型是浏览器提供的 DOM 的功能，而 JavaScript 只是调用 DOM 提供的事件接口。

## 二、EventTarget 接口
> DOM 的事件操作（监听与触发），都是定义在`EventTarget`接口，所有节点对象都部署了这个接口。该接口主要提供三个实例方法：
- addEventListener：绑定事件的监听函数
- removeEventListener：移除事件的监听函数
- dispatchEvent：触发事件
### 1. addEventListener()
> 用于在当前节点或对象上，定义一个特定事件的监听函数，一旦这个事件发生，就会执行监听函数，无返回值
#### 1.1 参数
- type：事件名，大小写敏感
- listener：监听函数
- useCapture：布尔值（默认 false），表示监听函数是否在捕获阶段触发，不写或 false 表示只在冒泡阶段触发
#### 1.2 多个监听
addEventListener 可以为当前对象的同一个事件，添加不同的监听函数，先添加先触发；如果同一个事件多次添加同一个监听函数，该函数只会执行一次

### 2. removeEventListener()
> 用于移除 addEventListener 添加的事件监听函数，无返回值
1. 参数与 addEventListener 完全一致
2. 移除的监听函数，必须是 addEventListener 添加的那个监听函数，而且必须在同一元素节点，否则无效
```js
div.addEventListener('click', function(e){}, false)
div.removeEventListener('click', function(e){}, false)
// 无效，因为监听函数不是同一个匿名函数
p.addEventListener('mousedown', handleMouseDown, true)
p.removeEventListener('mousedown', handleMouseDown, false)
// 无效，因为参数三不一样
```

### 3. dispatchEvent()
> 在当前节点上触发指定事件，从而触发监听函数的执行。返回一个布尔值，只要有一个函数调用了`Event.preventDefault()`，则返回值为 false，否则为 true
1. 参数
- 该方法的参数是一个 Event 对象的实例
```js
div.addEventListener('click', hello, false)
let event = new Event('click')
div.dispatchEvent(event)
// 当前节点触发了 click 事件
```
2. 自定义事件
- 浏览器自带的事件非常之多，但是开发者也能在自带事件外，自定义事件
```js
button.addEventListener('click', ()=>{
    const event = new CustomEvent('abc', {
        detail: {name: 'abc', age: 18},
        bubbles: true,
        cancelable: false
    })
    button.dispatchEvent(event)
})
div.addEventListener('abc', ()=>{
    console.log('abc 事件触发了')
    console.log(e.detail)
})
```
- 自定义了名称为 'abc' 的事件，同时设置其自动冒泡，且不能阻止

## 三、监听函数
JS有三种方法，可以为事件绑定监听函数
### 1. HTML 的 on- 属性
```html
<body onload="doSomething()"></body>
<div onclick="console.log('触发事件')"></div>
// 注意，这些属性的值是将会执行的代码，而不是一个函数
<body onload="doSomething()"> // 正确
<body onload="doSomething"> // 错误
```
- 一旦指定事件发生，on- 属性的值会原样传入 JS 引擎，因此如果要执行函数，不要忘记添加圆括号
- 这个绑定监听函数的方法，只会在冒泡阶段触发

### 2. 元素节点的事件属性
```js
window.onload = doSomething
div.onclick = function(event) {
    console.log('触发事件')
}
```
- 这个方法与 HTML 的 on- 属性的差异是，它的值为函数名（doSomething）；而后者，必须给出完整的监听代码（doSomething()）
- 只会在冒泡阶段触发

### 3. addEventListener()
> 所有 DOM 节点实例都有 `addEventListener` 方法，用来为该节点定义事件的监听函数

### 4. 小结
- HTML 的 on- 属性方法，违反代码分离原则，不推荐
- 元素节点事件属性，同一事件只能定义一个监听函数，如定义两个`click`事件，后一次定义会覆盖前一次，不推荐
- addEventListener，同一事件可添加多个监听函数，能够指定在哪一阶段触发，它是整个 JavaScript 统一的监听函数接口

## 四、this 的指向
> 监听函数内部的 this 指向触发事件的那个元素节点
```js
// 写法一
<button id ="btn" onclick="console.log(this.id)"> 点击 </button>
// 写法二
<button id="btn">点击</button>
let btn = document.querySelector('#btn')
btn.onclick = function() {
    console.log(this.id)
}
// 写法三
btn.addEventListener(
    'click',
    function(e) {
        console.log(this.id)
    },
    false
)
// 以上三种写法，点击按钮输出的都是 btn
```

## 五、事件的传播
### 1. 从一段代码看起
```html
<div class="grandfather">
    <div class="father">
        <div class="son">
        文字
        </div>
    </div>
</div>
// 分别为三个 div 的点击事件绑定了监听函数 fnYe、fnBa、fnEr
```
- 问题1：点击了文字，算不算点击了爷爷、爸爸、儿子？
- 问题2：点击了文字，最先调用哪个监听函数？
- 答案：都算；都行

### 2. 标准
1. 历史上，点击文字后，IE5 认为先调用 fnEr，网景认为先调用 fnYe
2. 2002年，W3C 发布标准，规定浏览器应该同时支持两种调用顺序：
- **首先**按**爷爷 -> 爸爸 -> 儿子**顺序看有没有函数监听，有就调用并提供事件信息，没有就跳过
- **然后**按**儿子 -> 爸爸 -> 爷爷**顺序看有没有函数监听，有就调用并提供事件信息，没有就跳过
3. 术语
- **从外向内**找监听函数，叫**事件捕获**
- **从内向外**找监听函数，加**事件冒泡**
4. 疑问
- 那岂不是 fnYe、fnBa、fnEr 都要调用两次？
- 不是这样的，开发者可以自己选择，把监听函数调用放在冒泡阶段，还是放在捕获阶段，默认是在冒泡阶段调用监听函数
- 具体实现是通过设置 `addEventListener()` 的参数三
- 注意，在冒泡阶段调用监听函数，不是说捕获阶段就不走了，两个阶段都走，只是说在冒泡阶段询问调用监听函数
5. 图示

![捕获冒泡示意图.jpg](https://i.loli.net/2021/01/15/Bc9s6Lbw2fMgWqY.jpg)
![你可以选择把fn放在哪边.jpg](https://i.loli.net/2021/01/15/8BrMAQ2hLcfs9pl.jpg)

6. 一个特例
- 只有一个元素被监听（即不考虑父子同时被监听），fn 分别在捕获和冒泡阶段监听事件，哪一个监听函数先被添加就先执行哪一个

### 3. target V.S. currentTarget
- event.target：用户操作的元素
- event.currentTarget：开发者监听的元素

### 4. 阻止冒泡与阻止默认行为
1. 所有“冒泡”都可以取消，使用 `event.stopPropagation()` 方法，可以阻止事件触发向上传播，但是不会阻止该元素上的其他监听函数的触发
2. 默认行为，如点击超链接跳转、滑动滚轮控制滚动条等。有的默认行为可以通过 `event.preventDefault()` 方法来取消，有的则不可以。前提是事件的 `cancelable` 属性为 true

## 参考资料
- [DOM 事件模型](https://wangdoc.com/javascript/events/index.html)