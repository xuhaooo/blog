> jQuery 是目前全球使用最广泛的 JavaScript 函数库，虽然目前很多公司和个人开发已经很少在主用 jQuery 了，但是它对很多 DOM 操作的封装以及各种设计模式，都是非常值得我们去学习的。

## 一、如何选择网页元素
> 如果用一句话来概括 jQuery 的功能，那就是：
> “选择某个网页元素，然后对其进行某种操作”
那么如何选择一个元素？将一个选择表达式放入构造函数 jQuery()（简写 $），即可得到被选中的元素。
1. 选择表达式可以是 CSS 选择器
```js
$('#myId') // 选择 id 为 myId 的元素
$('div.myClass') // 选择类名为 myClass 的 div 元素
```
2. 选择表达式也可以是 [jQuery特有的表达式](https://api.jquery.com/category/selectors/)
```js
$('a:first') // 选择网页中第一个 a 元素
$('#myForm :input') // 选择表单中的 input 元素
$('div:gt(2)') // 选择所有的 div 元素，除了前三个
```

## 二、对选择的网页元素进行过滤
1. 对选择的元素进行过滤（筛选）
```js
$('div').has('p') // 选择包含 p 元素的 div 元素
$('div').first() // 选择第一个 div 元素
$('div').not('.myClass') // 选择类名不是 myClass 的 div 元素
```
2. 对选择元素的附近元素的过滤
```js
$('div').next('p') // 选择 div 后面的第一个 p 元素
$('div').parent() // 选择 div 元素的父元素
$('div').siblings() // 选择 div 元素的同级元素
```

## 三、链式操作是个啥
> jQuery 的精华之一，就是可以在选择元素之后，可以像链条一样对元素进行一系列的连续操作，每一个操作所针对的元素可以相同也可以不同
```js
$('div').find('h2').eq(2).html('World').end().eq(0).html('Hello')
// .eq(2) 对 h2 元素进行过滤，即选择第二个 h2 元素
// .end() 使选择的结果后退一步，即退回到选中所有 h2 元素那一步
```
**原理：**每一步操作都是返回一个 jQuery 对象，所以不同的操作可以连在一起

## 四、怎么对元素进行取值和赋值
> jQuery 中使用同一个函数对元素进行取值和赋值，具体是取值还是赋值，通过参数来决定
```js
$('h3').html() // .html() 没有参数表示取 h3 的值
$('h3').html('Title3') // .html() 有参数 Title3，表示对 h3 进行赋值
```
1. 常见的取赋值函数有
- .text()
- .attr()
- .val()
2. 注意
- 如果选择元素的结果是多个，那么赋值对所有结果都有效
- 而取值只对结果的第一个元素有效，`.text()`除外

## 五、对网页元素的移动（2种视角）
> 效果相同，但是差别在于，不同方式返回的元素不同
1. 直接移动该元素，如
```js
$('div').insertAfter($('p')) // 直接把 div 移动到 p 后面（返回 div）
```
2. 移动其他元素，使目标元素到达想要的位置，如
```js
$('p').after($('div')) // 把 p 加到 div 前面（返回 p）
```
3. 相似的方法有：
```js
.insertBefore()/.before() // 移动到元素前面
.appendTo()/append() // 移动到元素的内部的后面
.prependTo()/.prepend() // 移动到元素内部的前面
```

## 六、元素的创建、复制、删除
1. 创建元素
```js
// 将新元素传入 jQuery 构造函数即可
$('<p>Hello World!</p>')
$('ul').append('<li>list item</li>')
```
2. 复制元素`.clone()`
3. 删除元素
```js
.remove() // 不保留被删除元素的事件
.detach() // 保留被删除元素的事件
.empty() // 清空元素内容（不删除该元素）
```

## 七、还有一些工具方法
> jQuery 除了可以对选中元素进行操作外，还提供了一些工具方法，这些方法定义在 jQuery 构造函数上，可以直接使用；而上面提到的操作元素的方法，是定义在 jQuery 的原型上面的，必须生成实例（选中元素）之后，才能使用
- 常见的工具方法有
```js
$.trim() // 清除字符串两端空格
$.each() // 遍历一个数组或对象
$.extend() // 将多个对象合并到第一个对象
$.makeArray() // 将对象转化成数组
$.type() // 判断对象的类别（函数对象、数组对象、日期对象等）
```

## 八、怎么进行事件操作
1. jQuery 直接把事件绑定在网页元素上。类似于 DOM API，所有事件处理函数都可以接受一个事件对象（event object）作为参数
```js
$('p').click(function(e){
    alert(e.type) // "click"
})
```
2. jQuery 支持的事件在 jQuery 内部都是`.bind()`的便捷方式，使用`.bind()`可以更灵活的控制事件，如为多个事件绑定同一个函数
```js
$('input').bind(
    'click change', // 同时绑定 click 和 change 事件
    function(){
        alert('Hello')
    }
)
```
3. 事件运行一次与事件解绑
```js
$.('p').one("click", function(){
    alert('Hello') //只运行一次，之后点击不会运行
})

$('p').unbind('click')
```
4. 事件处理函数中，可以用 this 关键字返回事件针对的 DOM 元素

## 参考资料
- [jQuery的设计思想](http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)
- [jQuery中文文档](https://www.jquery123.com/)