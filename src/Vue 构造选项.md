## 一、Vue 实例

什么是 Vue 实例？

我们在使用 jQuery 的时候，用 jQuery 去选一个元素，会直接得到这个元素吗？

并不会，而是得到了一个 jQuery 实例对象，这个对象里面封装了对那个元素的所有操作

相似的，`const vm = new Vue(options)` 就是构造一个 Vue 实例

这个实例它会去做什么呢，它会根据你给出的选项（options）得出一个对象（vm）

这个 vm 对象封装了这个 DOM 元素以及对应的所有操作

即，不管是事件绑定、数据读写、DOM 更新等全部由 vm 负责，我们只需要调用它的 API 即可

进一步的，根据原型的知识，我们在搞懂以下 5 个问号就可以对 Vue 有了充分的了解

![](https://i.loli.net/2021/03/09/ZQFmqRJldX1V59o.jpg)

首先，我们来看一下第一个问号，即 options

options 是 new Vue 的参数，一般称之为选项或构造选项

## 二、options 里面有什么



### 1. 数据

data、props、propsData、computed、methods、watch

### 2. DOM

el、template、render、renderError

### 3. 生命周期钩子

beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、activated、deactived、beforeDestroy、destroyed、errorCaptured

### 4. 资源

directives、filter、components

### 5. 组合

parent、mixins、extends、provide、inject

### 6. 其他

## 三、入门属性

由于 Vue 的options 选项的内容较多，先来总结一下比较简单好理解的一些选项

- el - 挂载点

  可以用 $mount 代替

- data - **内部**数据

  支持对象和函数，优先使用函数

- methods - 方法

  事件处理函数或者是普通函数

- components

  Vue 组件，注意大小写

  三种引入方式，推荐第一种

  

- 四个钩子

  created - 实例出现在内存中

  mounted - 实例出现在页面中

  updated - 实例更新了

  destroyed - 实例从页面和内存中消亡了

- props **外部**数据

  也就属性

  message="n" 传入字符串

  :message="n" 传入 this.n 数据

  :fn="add" 传入 this.add 函数

### 1. el - 挂载点

- 即，你想挂载到哪个节点（需要跟页面中你想挂载的节点保持一致）

- 挂载点页面节点里面的内容，会被替换掉，如果你在里面写了内容，用户基本上是看不到的，除非网速非常慢

- 可以用$mount 代替

```JavaScript
new Vue({
  el: '#app',
  data(){
    return {n: 1}
  }
})

new Vue({
  data(){
    return {n: 1}
  }
}).$mount('#app')
```

### 2. data - 内部数据

- 形式上支持对象和函数，优先使用函数

```JavaScript
new Vue({
  data: {
    n: 1
  }
})

new Vue({
  data(){
    return {n : 1} // 使用函数形式时，需要将数据包在一个对象里返回
  }
}).$mount('#app')
```

为什么推荐优先使用函数的形式呢？

举个例子，我们写一个 Demo.vue 组件

这个 Demo 组件实际上是一个对象，Vue 会自动的把这个对象传给 `new Vue`

```JavaScript
new Vue({
  render: h => h(Demo)
})
即
new Vue(Demo)
```

如果只传一次没什么问题，但如果两次使用 Demo 组件呢

这就意味着要传两次 Demo 给 new Vue

同一个对象，如果 data 是对象的形式，那就是同一个 data

两个组件引用的是同一个 data，这样会导致一个组件中修改了 data 会影响另外一个组件

因此，不推荐使用对象的形式，那为什么使用函数的形式没有问题呢

因为传值的时候发现 Demo 中的 data 是一个函数，那么就会调用这个函数，函数里返回的是全新的对象，即全新的 data

总之，尽量使用函数形式的 data

### 3. methods - 方法

- 事件处理函数或者普通函数(必须写到 methods 里面否则会报错提示函数未定义)

- 特性：每次渲染一次都会执行，即使执行的结果是一样的、毫无意义的

用法 1：事件处理函数

用法 2：普通函数，可以用来主动在模板里面调用

```JavaScript
// 可以用来替代 Vue 的 filter
new Vue({
  data(){
    return {array: [1, 2, 3, 4, 5, 6]}
  },
  template: `
    <div>
      {{filter()}} // 这个不是 Vue 的 filter而是 JS 的 filter
    </div>
  `,
  methods: {
    filter(){
      return this.array.filter(i => i % 2 === 0)  
    }
  }
}).$mount('#app')
```

由于 methods 中的函数在每次渲染时都会执行，即使页面中改变了其他东西导致页面渲染，这个函数依旧会执行

这样的问题引出了 computed，之后再说

### 4. components

#### 创建方式1：直接写一个 .vue 文件，这个文件就是 Vue 组件

```JavaScript
// main.js
import Demo from './demo.vue'
new Vue({
  components: { // vm 中引入 Demo 组件
    Example: Demo
  },
  template: `
    <Example /> 
  `
}).$mount('#app')
```

其实这样写有点奇怪，明明我引入的是 Demo.vue 组件，为什么我引入使用的时候要给它换个名字呢

所以我们写成这样：`components: { Demo: Demo }`

ES6 中提供了一直优化写法，如果你左边右边相等，那么可以把右边省略

即 `components: { Demo }`

#### 创建方式 2：用 JS 的方式声明一个全局的组件

```JavaScript
// main.js
Vue.component('Demo2', {
  template: `<div>demo2</div>`
}) 

new Vue({
  template: `
    <Demo2 />
  `
}).$mount('#app')
```

这种方式可以做到，整个应用的全局组件都能使用 Demo2

不需要再 vm 中再引入 Demo2，就可以在 `template` 中直接使用

这种方式中，`Vue.component()` 的参数 2 ，跟 `new Vue()` 的参数相同

可以视作，这个组件就是给一个实例取一个名字

实际上实例和组件的区别是叫法不同，你是入口你就是实例，你被别人使用你就是组件

#### 创建方式 3：结合方式1和 2

```JavaScript
new Vue({
  components: {
    Example: {
      data(){ return { n: 0 } },
      template: `<div>Example's n: {{n}}</div>`
    }
  },
  template: `
    <Example />
  `
}).$mount('#app')
```

Example 组件的写法跟 options 选项一模一样

所以可以认为组件就是实例中的实例

当然你也可以把这个对象写在 `new Vue()` 外面（给个名字），然后在 vm 中引入使用

```JavaScript
const x = {
  data(){ return { n: 0 } },
  template: `<div>Example's n: {{n}}</div>`
}

new Vue({
  components: {
    Example: x
  },
  template: `
    <Example />
  `
}).$mount('#app')
```

#### 总结

- 三种引入方式，优先使用第一种，更加符合模块化

- 注意大小写问题，建议文件名全用小写，组件名用大写字母开头，即 demo.vue、Demo

### 5. 四个钩子

- created - 实例/组件出现在内存中

- mounted - 实例出现在页面中

- updated - 实例更新了

- destroyed - 实例从页面和内存中消亡了

```JavaScript
new Vue({
  created(){},
  mounted(){},
  updated(){},
  destroyed(){}
}).$mount('#app')
```

钩子，是一个函数，也是一个切入的机会

Vue 将一个实例/组件的创建到销毁的过程类比于人的一生

而钩子就是用来表示不同组件生命周期中不同状态的函数

我们可以利用钩子在组件不同的状态下执行我们给定的操作

### 6. props - 外部数据

也叫属性

- message = "n" - 传入字符串

- :message="n" - 传入 this.n 数据

- :fn="add" - 传入 this.add 函数

props 与 data 的区别在于，外部数据是外部传值的，内部数据是内部自己传值的

```JavaScript
// demo.vue
<template>
  {{message}}
</template>

<script>
export default {
  props: ['message']
}
</script>

<style scope>
</style> 

// main.js
import Demo from './demo.vue'
new Vue({
  data(){
    return {
      n: 1
    }
  },
  template: `
    <Demo message="hello"/>
  `
}).$mount('#app')
```

**传递字符串**

Demo 组件从外部接收一个 message，这个 message 会自动绑定到 Demo 的 this 上面

Vue 允许我们直接省略 this，所以我们可以在 template 中使用模板`{{message}}` 

即Demo组件内部通过props 来声明一个外部数据，并接受外部传入的数据

在 main.js 里面使用 Demo 组件的同时传递数据到 Demo 组件中，页面显示出 hello

**传递变量**

既然实例可以传递一个字符串参数给 Demo 组件，那能否将实例的data 传递给组件呢

如果直接使用 `<Demo message="n"/>` 那么传递进去的就仅仅是字符串 `"n"` 

而是使用 `<Demo :message="n"/>` 其中 "" 中间的就是 JS 代码

即变量，变量从哪里找呢，当然优先从 data 上找

**传递方法**

那是否能传递方法进去呢

```JavaScript
// demo.vue
<template>
  {{message}}
  <button @click="fn">call fn</button>
</template>
<script>
export default {
  props: ['fn']
}
</script>
<style scope>
</style> 

// main.js
import Demo from './demo.vue'
new Vue({
  data(){
    return {
      n: 1
    }
  },
  template: `
    {{n}}
    <Demo :fn="add"/>
  `,
  methods: {
    add(){this.n += 1}
  }
}).$mount('#app')
```

将 add 方法通过 fn 从实例传递到 Demo 组件中

点击按钮实例中的 n 会实现加一功能

不过这个点击加一的函数是从实例传递到 Demo 组件，是组件调用改变实例上的数据

**同时传递变量和方法**

```JavaScript
<template>
  {{message}}
  <button @click="fn">call fn</button>
</template>
<script>
export default {
  props: ['message', 'fn']
}
</script>
<style scope>
</style> 

// main.js
import Demo from './demo.vue'
new Vue({
  data(){
    return {
      n: 1
    }
  },
  template: `
    {{n}}
    <Demo :message="n" :fn="add"/>
  `,
  methods: {
    add(){this.n += 1}
  }
}).$mount('#app')
```

实例和组件中的数字在点击按钮之后，会同时变化