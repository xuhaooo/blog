## 对象
### 1. 定义
无序的数据集合，键值对的集合
### 2. 写法
- 写法1：let obj = { 'name': 'frank', 'age': 18 }
- 写法2：let obj = new Object({'name': 'frank'})
- console.log({ 'name': 'frank, 'age': 18 })
### 3. 细节
- 键名是字符串，不是标识符，可以包含任意字符
- 引号可省略，省略之后就只能写标识符
- 就算引号省略了，键名也还是字符串（重要）
### 4. 属性名
每个 key 都是对象的属性名（property）
### 5. 属性值
每个 value 都是对象的属性值

## 删除属性
### 1. delete obj.xxx 或 delete obj['xxx']
- 即可删除 obj 的 xxx 属性
- 注意区分「属性值为 undefined」和「不含属性名」
- obj.name === undefined / delete obj.name
### 2. 不含属性名
'xxx' in obj === false // 'xxx' 是不是 obj 的属性名
### 3. 含有属性名，但是值为 undefined
'xxx' in obj && obj.xxx === undefined
### 4. 注意：obj.xxx === undefined
不能断定 'xxx' 是否为 obj 的属性


## 查看所有属性
### 1. 查看自身所有属性
- Object.keys(obj)，查看属性名
- Object.values(obj)，查看属性值
- Object.entries(obj)，查看属性名和属性值
- 缺陷就是，不会打印出共有属性
### 2. 查看自身+共有属性
- console.dir(obj)
- 或者自己依次用 Object.keys 打印出 obj.__proto__
### 3. 判断一个属性是对象自身的还是共有的
- in 操作符不会区分自身还是共有，如 'toString' in obj // true
- 用 obj.hasOwnProperty('toString') // false

## 查看单独属性
### 1. 两种方法查看属性
- 中括号语法：obj['key']
- 点语法：obj.key
- 坑新人语法：obj[key] // 变量 key 值一般不为 'key'
- 即如果[]里面是变量，你必须先求变量的值
### 2. 注意优先使用中括号语法
- 点语法会误导你，让你以为 key 不是字符串
- 等你确定不会弄混两种语法，再改用点语法


## 修改或增加属性
### 1. 直接赋值
- let obj = {name: 'frank'} // name 是字符串
- obj.name = 'frank' // name 是字符串
- obj['name'] = 'frank'
- ~obj[name] = 'frank'~ // 错，因 name 值不确定
- obj['na'+ 'me'] = 'frank'
- let key = 'name';obj[key] = 'frank'
- let key = 'name';~obj.key = 'frank'~ // 错
- 因为 obj.key 等价于 obj['key']
### 2. 批量赋值
Object.assign(obj, {age:18,gender:'man'})

## 修改或增加共有属性
### 1. 无法通过自身修改或增加共有属性
- let obj = {},obj2 = {} // 共有 toString
- obj.toString = 'xxx' 只会在改 obj 自身属性
- obj2.toString 还是在原型上
### 2. 我偏要修改或增加原型上的属性
- obj.__proto__.toString = 'xxx' // 不推荐使用 __proto__
- Object.prototype.toString = 'xxx'
- 一般来说，不要修改原型，会引起很多问题 


## 修改隐藏属性（原型链）
### 1. 不推荐使用 __ptoto__
```js
let obj = {name:'frank'}
let obj2 = {name:'jack'}
let common = {kind:'human'}
obj.__proto__ = common
obj2.__proto__ = common
```
### 2. 推荐使用 Object.create
```js
let obj = Object.create(common)
obj.name = 'frank'
let obj2 = Object.create(common)
obj2.name = 'jack'
```
- 一般创建一个对象，let obj = {}，这样它就指向了 Object.prototype 了，那我不这么创建
- let obj = Object.create(common)，即以 common 为原型来创建对象 obj，然后后面加属性
- 当然也可以结合 Object.assign(obj,{}) 来批量添加属性


## 'name' in obj和obj.hasOwnProperty('name') 的区别
- 'name' in obj：只能看 'name'是不是 obj 的属性，不能区分是自身属性还是共有属性
- obj.hasOwnProperty('name')：可以查看该属性是不是对象的自身属性

