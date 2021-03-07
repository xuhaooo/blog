
## 一、虚拟 DOM 是什么
虚拟 DOM 与 「真实 DOM」 对应，即虚拟的 DOM。

## 二、虚拟 DOM 的优点
### 减少 DOM 操作
- 虚拟 DOM 可以将多次操作合并为一次操作，比如你添加 1000 个节点，却是一个接着一个操作的（减少频率）
- 虚拟 DOM 借助 DOM diff 可以把多余的操作省掉，比如你添加 1000 个节点，其实只有 10 个是新增的（减少范围）
### 跨平台
- 虚拟 DOM 不仅可以变成 DOM，还可以变成小程序、ios 应用、安卓应用，因为虚拟 DOM 本质上只是一个 JS 对象


## 三、虚拟 DOM 长什么样子
### React 
```js
const vNode = {
    key: null,
    props: {
        children: [ // 子元素们
            {type: 'span', ...},
            {type: 'span', ...}
        ],
        className: "red" // 标签上的属性
        onClick: ()=>{} // 事件
    },
    ref: null,
    type: "div", // 标签名 or 组件名
}
```
### Vue
```js
const vNode = {
    tag: "div", // 标签名 or 组件名
    data: {
        class: "red", // 标签上的属性
        on: {
            click: ()=>{} // 事件
        }
    },
    children: [ // 子元素们
        {tag: "span", ...},
        {tag: "span", ...}
    ],
    ...
}
```


## 四、如何创建虚拟 DOM
### React.createElement
```js
createElement('div',{className:'red',onClick:()=>{}},[
    createElement('span', {}, 'span1'),
    createElement('span', {}, 'span2')
    ]
)
```
### Vue（只能在 render 函数里得到 h）
```js
h('div', {
    class: 'red',
    on: {
        click: ()=>{}
    },
}, [h('span', {}, 'span1'), h('span', {}, 'span2')])
```


## 五、用 JSX 简化创建虚拟 DOM
### React JSX
```js
<div className="red" onClick="{()=>{}}">
    <span>span1</span>
    <span>span2</span>
</div>
```
### Vue Template
```js
h('div',{
    class: 'red',
    on: {
        click: ()=>{}
    },
}, [h('span', {}, 'span1'), h('span', {}, 'span2')])
```


## 六、现在创建虚拟 DOM 的方法
### React
```js
<div className="red" onClick={fn}>
    <span>span1</span>
    <span>span2</span>
</div>
```
- 通过 babel 转为 createElement 形式

### Vue Template
```js
<div class="red" @click="fn">
    <span>span1</span>
    <span>span2</span>
</div>
```
- 通过 vue-loader 转为 h 形式


## 七、总结
### 1. 虚拟 DOM 是什么
- 一个能代表 DOM 树的对象，通常含有**标签名、标签上的属性、事件监听、子元素们**，一起其他属性
### 2. 虚拟 DOM 有什么优点
- 能减少不必要的 DOM 操作（两个例子背下来）
- 能跨平台渲染
### 3. 虚拟 DOM 有什么缺点
- 需要额外的创建函数，如 createElement 或 h，但可以通过 JSX 来简化为 XML 写法


## 八、DOM diff
### 1. 把虚拟 DOM 想象成树形
```js
<div :class="x">
    <span v-if="y">{string1}</span>
    <span>{string2}</span>
</div>
```
![把虚拟 DOM 想象成树.jpg](https://i.loli.net/2021/03/05/NQmR7JqELABDdub.jpg)

### 2. 当数据变化时，x 从 red 变成 green
![x 从 red 变成 green.jpg](https://i.loli.net/2021/03/05/il9dDTvSaosRknX.jpg)
#### DOM diff 发现
- div 标签类型没变，只需要更新 div 对应的 DOM 的属性
- 子元素没变，不更新

### 3. 当数据变化时，y 从 true 变成 false
![y 从 true 变成 false.jpg](https://i.loli.net/2021/03/05/SFilgKkpVOZhD5w.jpg)
#### DOM diff 发现
- div 没变，不用更新
- 子元素 1 标签没变，但是 children 变了，更新 DOM 内容
- 子元素 2 不见了，删除对应的 DOM

## 九、总结
### 什么是 DOM diff
- 就是一个函数，我们称之为 patch
- patches = patch(oldVNode, newVNode)
- patches 就是要运行的 DOM 操作，可能长这样：
```js
[
    {type: 'INSERT', vNode:...},
    {type: 'TEXT', vNode:...},
    {type: 'PROPS', propPatch:[...]}
]
```


## 十、DOM diff 可能的大概逻辑
### 1. Tree diff
- 将新旧两棵树逐层对比，找出哪些节点需要更新
- 如果节点是组件就看 Component diff
- 如果节点是标签就看 Element diff
### 2. Component diff
- 如果节点是组件，就先看组件的类型
- 类型不同直接替换（删除旧的）
- 类型相同则只更新属性
- 然后深入组件做 Tree diff（递归）
### 3. Element diff
- 如果节点是原生标签，则看标签名
- 标签名不同直接替换，相同则只更新属性
- 然后进入标签后代做 Tree diff（递归）


## 十一、DOM diff 的缺点
### 同级节点对比存在 bug
- 会出现识别错误的问题
- DOM diff 中的 key 问题
- [Vue2.0 v-for 中 :key 到底有什么用](https://www.zhihu.com/question/61064119/answer/766607894)
- 这个问题在 React 中也存在

## 深入阅读
- [React 虚拟 DOM 和 diff 算法](https://juejin.im/post/6844903529161850893)
- [React 源码剖析系列 - 不可思议的 react diff](https://zhuanlan.zhihu.com/p/20346379)