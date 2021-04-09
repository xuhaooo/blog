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

[Vue 文档 - 选项](https://cn.vuejs.org/v2/api/#vm-options)

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

- el - 挂载点

  可以用 $mount 代替

- data - **内部**数据

  支持对象和函数，有限用函数

- methods - 方法

  时间处理函数或者是普通函数

- components

  Vue 组件，注意大小写

  三种引入方式，推荐最后一种

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