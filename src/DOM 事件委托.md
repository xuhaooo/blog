> DOM 事件模型中，事件会在冒泡阶段向上传播，因此可以把子节点的监听函数定义在祖先节点上，由祖先节点的监听函数统一处理多个子节点的事件，这种方法叫做事件委托

## 一、从需求的角度来看
### 1. 场景一
- 需要给 100 个按钮添加点击事件，怎么解决？
- 方法一：当然可以通过循环来依次添加，但是带来的后果就是内存中有了 100 个监听器
- 方法二：通过事件委托，把监听函数放在这 100 个按钮的祖先节点上，等冒泡阶段时判断 target 是否是这 100 个按钮中的一个
```js
div.addEventListener('click', (e)=>{
    const el = e.target
    if(t.tagName.toLowerCase() === 'button'){
        console.log('button 被点击了')
        console.log('button 的内容是：' + el.textContent)
        // 通过设置 button 的 data-id 属性，还能获取 id
        console.log('button 的 data-id 是：' + el.dataset.id)
    }
})
```

### 2. 场景二
- 需要监听目前还不存在的按钮元素的点击事件，怎么解决？
- 方法：通过事件委托，监听祖先元素，等点击的时候判断是否是我想要监听的元素即可
```js
div.addEventListener('click', (e)=>{
    const el = e.target
    if(t.tagName.toLowerCase() === 'button'){
        console.log('button 被点击了')
    }
})
```

### 3. 优点
- 省监听数（内存）
- 可以监听动态元素

## 二、封装事件委托
### 1. 需求
- 写出这样一个函数：`on('click', '#testDiv', 'button', fn) {}`，当用户点击 `#testDiv` 里面的 `button` 元素时，调用 `fn` 函数；要求使用事件委托

### 2. 答案一
- 通过事件委托监听祖先元素，然后判断当前的 `target` 是否满足选择器，如果满足就调用函数
```js
setTimeout(()=>{
    const button = document.createElement('button')
    button.textContent = 'click1'
    div.appendChild(button)
})

on('click', '#testDiv', 'button', ()=>{
    console.log('button 被点击了')
})

function on(eventType, element, selector, fn){
    // 如果参数不是一个元素，那就去找这个元素
    if(!(element instanceOf Element)){
        element = document.querySelector(element)
    }
    element.addEventListener(eventType, (e)=>{
        const el = e.target
        if(el.matches(selector)){
            fn(e)
        }
    })
    return element
}
```

### 3. 答案二
- 上述方法，在面对以下情况时会失效：
- 如果将文字放入 `span` 中，再将 `span` 放入 `button`中，然后在 `div` 上监听 `button`
- 原因在于操作的元素是 `span`，而监听的元素是 `button`，并不匹配
- 优化的方案思路是：通过递归判断祖先中有没有 `button`，只要有一个就表明我点击了按钮，那么就调用函数
```js
on('click', '#testDiv', 'button' ()=>{
    console.log('button 被点击了')
})

function on(eventType, element, selector, fn){
    if(!(element instanceOf Element)){
        element = document.querySelector(element)
    }
    element.addEventListener(eventType, (e)=>{
        const el = e.target
        while(!el.matches(selector)){
            // 边界情况，不能向上超出 testDiv
            if(element === el){
                el = null
                break
            }
            el = el.parentNode
        }
        el && fn.call(el, e, el) // 传入 this、e、匹配元素
    })
    return element
}
```

## 参考资料
- [DOM 事件委托](https://wangdoc.com/javascript/events/model.html)


