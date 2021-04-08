> 我们通过完成 3 个目标来达到对 Vue 有个基本的认识

## 一、目标一：搭建出一个使用 Vue 的项目

想要搭建出一个使用 Vue 的项目，我们可以使用 2 种方法：

### 1. 方法一：使用 @vue/cli

搜索 @vue/cli，进入官网打开文档，打开「创建一个项目」章节：

- 全局安装：`yarn global add @vue/cli`

- 创建目录：`vue create 路径（目录名）`

你可以在一个目录路径下运行 `vue create 项目名`直接创建项目目录

或新建一个项目目录然后进入该目录运行 `vue create .` 直接在当前目录内创建项目

- 进行安装配置（配置建议见下图）

- 进入项目目录，`yarn serve` 开启 webpack-dev-server 进行项目预览

- 用编辑器打开项目目录进行开发

![Vue选项.jpg](https://i.loli.net/2021/03/08/W1PafdKF9jDM5Eg.jpg)

![Vue选项续.jpg](https://i.loli.net/2021/03/08/diouEyTvecHrFIW.jpg)

> 以上配置选项只适合入门学习使用，如果是真实项目请自行斟酌；如遇到图中没有的配置项请选择默认选项；空格切换选中/不选，回车确认；如发现警告请忽略，如发现错误请自行 Google 解决

### 2. 方法二、使用打包工具从零搭建 Vue 项目

- 使用 Webpack 或者 rollup 从零开始

- 不适合新手

## 二、目标二：做一个最简单的项目

### 1. 简单尝试

> 需求：实现页面中有一个数字 1 和一个 +1 按钮，点击 +1 按钮后数字就加一

1. 第一步，我们可以尝试性的改一改几个主要文件里的内容

2. 比如，将 src/App.vue 文件更改为

```JavaScript
// App.vue
<template>
  <div id="app">
    {{n}}
    <button @click="add">+1</button>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  data() {
    return {
      n: 0
    }
  },
  methods: {
    add() {
      this.n += 1
    }
  }
}
</script>

<style lang="scss">

</style>  
```

1. 打开页面发现页面上已经实现了上述需求

但是你可能会有很多疑惑，比如 `{{}}` 、 `@click="add"`、data 函数、methods对象这些都是什么啥？

带着这些疑问我们去看 Vue 官方文档寻找答案，我们会看到“Vue 实例”

### 2. Vue 实例 —— 你必须需要了解的知识

通过上面的需求的例子很难看出什么是 Vue 实例

不过，我们先来看一下 src/main.js （入口文件）

里面有段代码是`new Vue({..})` 这是啥

还记得 jQuery 实例吗，我们通过 `const $div = new jQuery('#xxx')` 创建一个 jQuery 实例

不过 jQuery 通过一些技巧把 `new` 给省略了

它返回了什么？返回了一个封装对象，这个 `$div`对象封装了对 `#xxx` 的所有操作

相似的，这里我们可以写成 `const vm = new Vue({})` 只不过为了方便可以省略前面的 `const vm = ` 

这段代码返回了一个封装对象 vm 即 Vue 实例，里面封装了对这个实例对于页面内容的所有操作，包括数据、组件、事件绑定等等

这个时候我们可以做一下小小的修改，把 `new Vue()` 里面的对象删掉给它传一个空对象

打开浏览器发现控制台有报错提示， "run-time only...”，这是什么意思

查询Vue 文档安装篇，我们得知 Vue 存在完整版与运行时版的区别

接下来我们通过 3 种使用 Vue实例的方式来理解什么是 Vue 实例，以及体验一下 Vue 这两种版本的区别

### 3. 如何使用Vue实例

#### 方法一：从 HTML 得到视图

这种方式需要使用「Vue完整版」

引入 vue.js / vue.min.js 的 CDN 链接

这个完整版怎么用呢

我们可以直接把 main.js 中的代码全部注释然后 `console.log(Vue)` ，可以在控制台得到 Vue，然后编写几行代码

```HTML
 <!-- index.html -->
 <div id="app"> 
   {{n}}
 </div> 
```

```HTML
// main.js
new Vue({
  el: "#app", // 即我要对页面中的 id=app 的这个 div 元素进行一个 MVC 封装，可以直接从 HTML 里面获取到这个元素
  data: {
    n: 1
  }
}) 
```

这样可以在页面中看到一个 1

这个例子的关注点在于：我的视图没有写在 JS 代码中，而是我把长什么样子直接写在页面当中

这就是完整版的功能：它可以直接在页面里直接把这个 n 变成 1

#### 方法二：用 JS 构建视图

这种方式需要使用「Vue runtime 版本」

将 index.html 中的 vue.js/vue.min.js 替换成 vue.runtime.js/vue.runtime.min.js

还是可以在控制台中得到 Vue，**但是**页面中的 1 没有了

这就是完整版和非完整版的区别：非完整版（runtime）不支持从 HTML 中获取视图

```JavaScript
new Vue({
  el: '#app',
  template: `
    <div>{{n}}</div> // 外面必须要有一层包裹
  `,
  data: {
    n: 1
  }
})
```

即使这样写，也不支持；当然，完整版是支持这样写的

非完整版，只要你是从 HTML 里面产生视图都不支持，不管你写在HTML 中还是写在 JS 的 template 中

完整版，只要你从 HTML 中产生视图都支持，写在HTML、JS 里的 template 里都行

那这个非完整版连在页面中显示一个 n 都不行，要它干嘛

事实上，非完整版想要在页面中渲染视图需要这样写

```JavaScript
new Vue({
  el: '#app',
  render(h){ // 这个参数是 Vue 传给你的，h 即 createElement
    return h('div', this.n)
  },
  data: {
    n: 1
  }
})
// 页面中成功的出现了 1 
```

如果我需要 +1 怎么办，那就需要加按钮并绑定点击事件监听函数了，怎么写呢？

```JavaScript
new Vue({
  el: '#app',
  render(h){
    return h('div', [this.n, h('button', { on: { click: this.add } }, '+1')])
  },
  data: {
    n: 1
  },
  methods: {
    add(){
      this.n += 1
    }
  }
})
```

即你必须要用一个 h 函数去把所有的元素给构造出来

---

可能到这里，小白可能会觉得完整版更好用，那非完整版存在的意义是什么呢

虽然使用完整版可以通过 HTML 得到视图

而非完整版却要使用非常麻烦的 h 的方式，但是这种方式却是对的，为什么呢

首先你有一段 HTML，它是字符串，里面可能混杂了 `{{n}}` ，"html {{n}}"

Vue 就需要把这段东西，变成 "html 1"，怎么变呢，直接替换吗，不太好

因为里面可能很多复杂的语句比如 v-if、v-for等，这些复杂语句，你用直接正则替换是做不到的

于是 Vue需要写一个编译器，它可以把含有复杂语句或占位符的这些东西变成真实的DOM 节点

于是，当你下次点击加一按钮的时候，它就知道你要改的是n

于是它就去改 DOM 节点，而不是重新把之前的东西编译一遍

这个东西就做编译器，有个特点就是复杂，因此就会占用较多的代码体积

根据 Vue 文档说明，非完整版比完整版要小大约30%的体积

即非完整版没有编译器，因此它也没有办法把HTML 变成节点

我们肯定是想要体积更小的版本，但是它却不具备通过 HTML 获取视图的功能，怎么办呢

使用Webpack，即方法三

#### 方法三：使用 vue-loader

我在一开始写的时候写成 HTML 的形式，`<div>{{n}}</div>`

然后Webpack通过 vue-loader ，把它转换成 `h("div", this.n)` 

而我这个 vue-loader 需要让用户下载吗，不需要

我在 yarn build 的时候就已经转换完了，这样就可以让用户下载只支持 h 函数的版本

这样我们就做到了两个事情：写HTML 形式的代码，用户下载的是 h 函数版本的代码，中间用 vue-loader 来转换一下

这样用户对 vue 的依赖就是可以使用体积小30%的不完整版

那我们怎么使用 vue-loader 呢？

首先，写一个Demo.vue 的文件

```JavaScript
// Demo.vue
<template> // 视图
  <div class="red">{{n}}</div>
  <button @click="add">+1</button>
</template> 

<script> // 视图外的其他选项
 export default {
   data(){ // 如果你使用 vue-loader，这里的 data 必须写成函数的形式
     return {
       n: 0
     }
   },
   methods: {
     add(){
       this.n += 1
     }
   }
 }
</script>

<style scoped> // CSS
.red {
  color: red;
}
</style> 
```

然后 vue-loader 就可以这个文件变成一个对象，之后我们就可以使用它了

```JavaScript
// main.js
import Demo form './Demo.vue'
console.log(Demo) // 可以一下打印出什么
console.log(Demo.render.toString())
 new Vue({
   el: '#app',
   render(h){
     return h(Demo)
   }
 })
```

打印出来的是一个对象，里面有一个 render ，render 里面是转换之后的代码

所以我们在 .vue 文件里面不需要自己写 render

这就是 Webpack 带来的便利，你可以以任意的方式组织一个对象（比如 .vue 文件）

只需要你最后可以通过一个 loader 把它还原成这个对象即可

以上称作 Vue 单文件组件

不过这样的做法，由于 index.html 中视图部分只有一个 div#app，SEO 不友好

### 4. Vue 实例总结

1. Vue 实例相似于 jQuery 实例 - 封装了对DOM、data 的所有操作

2. 操作 DOM - 无非就是监听事件、改变 DOM 等

3. 操作 Data - 无非就是增删改查等

4. 没有封装 Ajax - 使用 axios

### 5. SEO 友好

> 即搜索引擎优化

可以看做搜索引擎在不停的 curl 各个网站的页面内容（网页源代码）

但是如果我们都是使用 .vue 文件的形式编写视图等代码，再渲染到页面中

那么页面中只有div#app 这个空标签了，这样会使得 SEO 不友好

**解决办法**：给 curl 一些内容，将 meta-title、meta-description、meta-keyword、h1、a等标签事先写到页面里面，如果 div#app 中写了内容，后面会被 JS 渲染替换掉，但是这样做会使得 SEO 更加友好

## 目标三：理解 Vue 的完整版与运行时版的区别

![](https://i.loli.net/2021/03/08/GjJqAWTHfvNclQu.jpg)

**最佳实践**：总是使用非完整版，然后配合 vue-loader 和 vue 文件

1. 保证用户体验，用户下载的 JS 文件体积更小，但**只支持 h 函数**

2. 保证开发体验，开发者可直接在 Vue 文件里写 HTML 标签，而**不写 h 函数**

3. 脏活让 loader 做，vue-loader 把 vue 文件里的 HTML **转为 h 函数**

