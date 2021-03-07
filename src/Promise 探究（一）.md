## 一、由一个问题引发的思考
### 1. 问题
- 之前在《JS 异步编程初探》提到的异步任务都是得到一个结果
- 那如果异步任务得到的结果有两个，比如成功和失败，该怎么办呢？
### 2. 两个解决办法
#### 方法一：回调接受两个参数
```js
fs.readFileSync('./1.txt', (error, data) => {
    if(error){
        console.log('失败了')
    }
    console.log(data.toString())
})
```
#### 方法二：搞两个回调
```js
ajax('GET', './1.json', data=>{}, error=>{})
// 或
ajax('GET', './1.json', {
    success: () => {},
    fail: () => {}
})
```
### 3. 这两个方法的问题
- 1. 两个回调函数名没有规范，有人用 `success+error`，有人用 `success+fail` 等
- 2. 很难进行错误处理
- 3. 容易出现回调地狱，代码可读性差
- 那有什么办法解决这些问题呢，使用 `Promise`

## 二、Promise 对象
### 1. 概述
> Promise 对象是 JavaScript 的异步操作解决方案，为异步操作提供统一接口。它起到代理作用（proxy），充当异步操作与回调函数之间的中介，使得异步操作具备同步操作的接口。Promise 可以让异步操作写起来，就像在写同步操作的流程，而不必一层层地嵌套回调函数
- Promise 是一个对象，也是一个构造函数，它接受一个回调函数作为参数，并返回一个 Promise 实例
- Promise 实例有一个 `then` 方法，用来指定下一步的回调函数
```js
function f1(resolve, reject) {
    // 异步代码
}
let promise = new Promise(f1)
promise.then(f2)
```

## 三、Promise 对象的状态
Promise 对象通过改变自身的状态，来控制异步操作。Promise 实例的状态有：
- 异步操作未完成（pending）
- 异步操作成功（fulfilled）
- 异步操作失败（rejected）

正如 Promise 的名字的意义，一旦状态发生变化，就不得再改变，这就意味着 promise 实例的状态变化只可能会发生一次。因此，Promise 的最终结果只有两种：
- 异步操作成功，Promise 实例传回一个值，状态变为 fulfilled
- 异步操作失败，Promise 实例传回一个错误，状态变为 rejected

## 四、怎么创建一个 Promise
JS 提供了原生的 Promise，用来生成 Promise 实例
```js
let promise = new Promise((resolve, reject) => {
    if(/*异步操作成功*/){
        resolve(value)
    } else { /*异步操作失败*/
        reject(error)
    }
})
```
- Promise 构造函数接收一个函数作为参数
- 函数分别有两个参数 resolve、reject，这两个函数参数由 JS 引擎提供，不用自己实现
- resolve 函数的作用是：将 Promise 实例的状态由“未完成”变为“成功”，在异步操作时调用，并将异步操作的结果作为参数传递出去
- reject 函数的作用是：将 Promise 实例的状态有“未完成”变为“失败”，在异步操作失败时调用，并将异步操作失败时报出的错误，作为参数传递出去


## 五、Promise.prototype.then()
- 接受两个回调函数，第一个是异步操作成功时的回调函数，第二个是异步操作失败时的回调函数（可省略）
- 一旦状态改变，就调用相应的回调函数
```js
let p1 = new Promise((resolve, reject) => {
  resolve('成功');
});
p1.then(console.log, console.error);
// "成功"

let p2 = new Promise((resolve, reject) => {
  reject(new Error('失败'));
});
p2.then(console.log, console.error);
// Error: 失败
```

## 六、Promise.all()
- 将多个 Promise 实例，包装成一个新的 Promise 实例并返回
```js
let p = Promise.all([p1, p2, p3])
```
- 其中 p1、p2、p3 都是 Promise 实例，如果不是就会将其转为 Promise 实例
- 只有当 p1、p2、p3 的状态都为成功（fulfilled），p 的状态才会变成成功(fulfilled)，并调用 resolve
- 只要参数中有一个 Promise 实例的状态为失败(rejected)，p 的状态都会变为失败（rejected），并调用 reject

## 七、Promise.race()
- 相似于 `Promise.all()`，将多个 Promise 实例包装成一个新的 Promise 实例并返回
```js
let p = Promise.race([p1, p2, p3])
```
- 不同点在于，只要 p1、p2、p3 中有一个率先状态发生改变，p 的状态就会发生改变
- 同时，最开始发生改变的那个 Promise 实例的返回值，会传给 p 的回调函数