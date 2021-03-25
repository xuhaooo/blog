我们通过完成 3 个目标来达到对 Vue 有个基本的印象

***

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

带着这些疑问我们去看 Vue 官方文档寻找答案

### 2. Vue 实例 —— 你必须需要了解的知识

通过上面的需求的例子很难看出什么是 Vue 实例

不过，我们先来看一下 src/main.js （入口文件）

里面有段代码是`new Vue({..})` 这是啥

还就得 jQuery 实例吗，我们通过 `const $div = new jQuery('#xxx')` 创建一个 jQuery 实例

不过 jQuery 通过一些技巧把 `new` 给省略了

它返回了什么？返回了一个封装对象，这个 `$div`对象封装了对 `#xxx` 的所有操作

相似的，这里我们可以写成 `const vm = new Vue({})` 只不过为了方便可以省略前面的 `const vm = ` 

这段代码返回了一个封装对象 vm 即 Vue 实例，里面封装了对这个实例对于页面内容的所有操作，包括数据、组件、事件绑定等等

这个时候我们可以做一下小小的修改，把 `new Vue()` 里面的对象删掉给它传一个空对象

打开浏览器发现控制台有报错提示， "run-time only...”，这是什么意思

于是我们得知 Vue 存在完整版与运行时版

接下来我们通过 3 种使用 Vue实例的方式来充分理解什么是 Vue 实例，以及体验一下 Vue 这两种版本的区别

### 3. 如何使用Vue实例

#### 方法一：从 HTML 得到视图

这种方式需要使用「Vue完整版」

可以通过 CDN 或 `import..` 引入 vue.js / vue.min.js

#### 方法二：用 JS 构建视图

这种方式需要使用「Vue runtime 版本」

#### 方法三：使用 vue-loader

## 目标三：理解 Vue 的完整版与运行时版的区别