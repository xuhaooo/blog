# CSS 动画总结

## 动画
### 1. 定义
由许多静止的画面（帧），以一定的速度（如每秒30张）连续播放时，肉眼因视觉残像产生错觉，而误以为是活动的画面
### 2. 概念
- 帧：每个静止的画面都叫做帧
- 播放速度：每秒 24 帧（影视）或者每秒 30 帧（游戏）
### 3. 原理
每过一段时间（用 setInterval 做到），将页面元素移动一段距离，直到移动到目标地点
### 4. 注意性能
> 控制台一个 tab => ESC => rendering，绿色闪烁，说明浏览器再重新渲染
- 绿色表示重新绘制（repaint）了
- CSS 渲染过程依次包含布局、绘制、合成
- 其中布局和绘制有可能被省略


## 前端高手不用 left 做动画
> 用 transform（变形）
### 1. 原理
`transform: translateX(0 => 300px)`
- 直接修改会被合成，需要等一会修改
- transition 过渡属性可以自动脑补中间帧
### 2. 注意性能
- 并没有 repaint（重新绘制）
- 比改 left 性能好


## 浏览器渲染器渲染过程
### 1. 过程
- 根据 HTML 构建 HTML 树（DOM）
- 根据 CSS 构建 CSS 树 （CSSOM）
- 将两棵树合成一颗渲染树（render tree）
- Layout 布局（文档流、盒模型、计算大小和位置）层叠上下文 素描
- Paint 绘制（把边框颜色、文字颜色、阴影等画出来）填色
- Composite 合成（根据层叠关系展示画面）拍到一起类似合并图层

### 三棵树
![三棵树.jpg](https://i.loli.net/2020/12/21/qe7IAzD5ouFZfyv.jpg)

> 第一次渲染是上面的过程，那我们要更新一下样式，又会发生什么呢

### 2. 如何更新样式
#### 一般我们用 JS 来更新样式
- 比如 div.style.background = 'red'
- 比如 div.style.display = 'none'
- 比如 div.classList.add('red')
- 比如 div.remove() 直接删掉节点
- 加样式不如加类，因为一个类里面可以有很多样式
#### 那么这些方法有什么不同吗
- 这些方法在渲染的时候都会走上面的那些步骤吗
-有三种不同的渲染方式，详细看下面

### 3. 三种更新方式
![三种更新方式.jpg](https://i.loli.net/2020/12/21/1WBHpkau8bEUf2o.jpg)
#### 第一种，全走
- div.remove() 会触发当前小时，其他元素 relayout
#### 第二种，跳过 layout
- 第二种方式就是，有的时候你可能不需要布局了，比如你没有改变一个元素的大小位置等，你只改了bg
- 改变颜色背景，直接 repaint + composite
#### 第三种，跳过 layout 和 paint
- 你没有该位置大小、颜色，比如我们只改了 transform，它只需要合成就行了
- 改变 transform，只需 composite
- 改变必须全屏查看效果，在 iframe 里看有问题


> 那么问题来了，有那么多的CSS样式<br>
> 我 TM 怎么知道，每个属性触发什么流程<br>
> CSS变态之处来了：自己一个一个试吧<br>
> 还好，程序员喜欢分享：https://csstriggers.com/ ，这个网站已经把所有的属性全部试过了


### 4. CSS 动画优化
#### 没什么技术含量
答案都在 Google 写的文章里，谁看完谁牛 X
5个步骤各有几句话，死记硬背就好了
#### JS 优化
使用 requestAnimationFrame 代替 setTimeout 或 setInterval
#### CSS 优化
使用 will-change 或 translate


## transform 变形
### 四个常用功能
- 位移 translate
- 缩放 scale
- 旋转 rotate
- 倾斜 skew
- 其他的看了没用，几乎不用
### 经验
- 一般都需要配合 transition 过渡
- inline 元素不支持 transform，需要先变成 block

### 1. transform 之 translate
#### 常用写法
- translateX(20px)
- translateY(20px)
- translate(20px,20px)
- translateZ(20px)且父容器 perspective
- translate3d(x,y,z)
- 单纯的translateZ，没有效果，三维世界没有近大远小啊
- 那怎么在二维的屏幕上模拟三维呢，你需要告诉我视点在哪，视野就是三个坐标轴的原点，怎么做呢
- 在父元素上加一个 perspective 属性，比如 1000px 即中间位置向屏幕里面往里 1000 像素，这也就会有效果了，比如你减小 translateZ 的数值，它就会往视点走，越来越远，抽象的说 translateZ 不是垂直于屏幕的方向而是垂直于视点的方向，发射
- 缩写： translate(x,y) translate3d(x,y,z)
#### 经验
- 要学会看懂 MDN 的语法实例
- translate(-50%,-50%) 可做绝对定位元素(left:50%,50%)的居中
- 不要写两份，因为会覆盖


### 2. transform 之 scale
#### 常用写法
- scaleX(2)
- scaleY(2)
- scale(2,2)
- number 原来的多少倍
- 没有动画
- 要动画效果的话需要在元素自身加上 transition
#### 经验
- 用的较少，因为容易出现模糊


### 3. transform 之 rotate
#### 常用写法
- rotate(10deg)
- rotateZ(10deg)
- rotateX(10deg)
- rotateY(10deg)
- rotate3d，太复杂，无法用语言表述
- 没有动画
- 默认以Z轴为轴心转动
#### 经验
- 一般用于 360 度旋转制作 loading
- 用到时再搜索 rotate MDN 看文档


### 4. transform 之 skew 
#### 常用写法
- skewX(20deg)
- skewY(20deg)
- skew(20deg,20deg)
#### 经验
- 用的较少
- 用到时再搜 skew MDN 文档


### 5. transform 多重效果
#### 组合使用
- transform: scale(0.5) translate(-100%,-100%);
- transform: none;取消所有


## transition 过渡
### 1. 作用
- 补充中间帧
- 你告诉它开头和结尾是怎么样的，中间它给你补齐
- 写在原始的元素上，而不是写在变化的元素上，因为变化的元素上你取消动作的时候它直接没有效果消失了没有回来动画，加在原始的元素上它来回都有动画
### 2. 语法
- transition: 属性名 时长 过渡方式 延迟
- transitio: left 200ms linear
- 可以用逗号分隔两个不同的属性
- transition: left 200ms, top 400ms
- 可以用 all 代表所有属性
- transition: all 200ms
- 过渡方式：linear匀速 | ease先快后慢 | ease-in | ease-out | ease-in-out | cubic-bezier | step-start | step-end | steps，具体含义要靠数学知识
### 3.注意
#### 并不是所有属性都能过渡
- display: block => none 没法过渡，怎么可能让一个东西看见到看不见过渡呢，可以用过渡啊，那你直接写opacity啊，不过opacity虽然会变透明但是它会继续占位置，所- 以一般都会用JS移除动画的类
- 一般改成 visibility: hidden => visible（不要问为什么）
- display 和 visibility 的区别自己搜一下
- background 颜色可以过渡吗？颜色可以，颜色的数值可以变换
- opacity 透明度可以过渡吗？
- 只要有过渡规律就可以


> 过渡必须要有起始 <br>
> 一般只有一次动画，或者两次 <br>
> 比如 hover 和非 hover 状态的过渡 <br>
> 如果除了起始，还有中间点怎么办

## 两种办法
### 使用两次 transform
- .a === transform ===> .b
- .b === transform ===> .c
- 如何知道到了中间点呢？
- 用 setTimeout 或者监听 transitionend 事件
### 使用 animation
- 声明关键帧
- 然后在关键帧上写动画效果
- 将其挂到元素动画启动元素里面的 animation 后面


## animation
### 缩写语法
- animation: 时长 | 过渡方式 | 延迟 | 次数 | 方向 | 填充模式 | 是否暂停 | 动画名;
- 时长：1s 或者 1000ms
- 过渡方式：跟 transition 取值一样，如linear
- 次数：3 或者 2.4 或者 infinite
- 方向：reverse反向 | alternate正反交替 | alternate-reverse
- 填充模式：none | forwards | backwards | both
- 是否暂停： paused | running 搜MDN的animation属性设置
- 以上所有属性都有对应的单独属性，改单独一个属性值的时候用，不然都是直接简写
