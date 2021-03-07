## 一、AJAX 发展史
### WEB 1.0 --- 没有 AJAX 的日子里
> 20世纪90年代，几乎所有的网站都由HTML页面实现，服务器处理每一个用户请求都需要重新加载网页。这样的处理方式效率不高，用户的体验是所有页面都会消失，再重新加载，即使只是一部分页面元素改变也要重新加载整个页面，不仅要刷新改变的部分，连没有变化的部分也要刷新，这会加重服务器的负担
### WEB 2.0
> 1999年，微软公司发布 IE 浏览器5.0版，第一次引入新功能：允许 JavaScript 脚本向服务器发起 HTTP 请求。这个功能当时并没有引起注意，直到2004年 Gmail 发布和2005年 Google Map 发布，才引起广泛重视。2005年2月，AJAX 这个词第一次正式提出，它是 Asynchronous JavaScript and XML 的缩写，指的是通过 JavaScript 的异步通信，从服务器获取 XML 文档从中提取数据，再更新当前网页的对应部分，而不用刷新整个网页。后来，AJAX 这个词就成为 JavaScript 脚本发起 HTTP 通信的代名词，也就是说，只要用脚本发起通信，就可以叫做 AJAX 通信。W3C 也在2006年发布了它的国际标准

## 二、如何使用
> AJAX 通过浏览器原生的 XMLHttpRequest 对象发出 HTTP 请求，得到服务器返回的数据后，再进行处理。具体步骤如下：
### 1. 创建 XMLHttpRequest 实例
```js
let request = new XMLHttpRequest()
```
- `XMLHttpRequest` 对象是 AJAX 的主要接口，用于浏览器与服务器之间的通信，虽然它的名字里面含有 XML 和 HTTP，实际上它可以使用多种协议，发送任何形式的数据
- `XMLHttpRequest` 本身是一个构造函数，可以使用 new 命令生成实例

### 2. 设置请求参数
```js
request.open('GET', 'https://www.example.com/users.json', true)   
```
- 实例创建之后，就可以通过 open 方法设置请求的参数

### 3. 指定回调函数，监听通信状态（readyState 属性值）的变化
```js
request.onreadystatechange = () => {
    if(request.readyState === 4 && request.status === 200){
        ...
    }
}
```
- 上面代码中一旦实例的状态发生了变化，就会调用箭头函数

### 4. 发送请求
```js
request.send()
```
- 一旦拿到了服务器返回的数据，AJAX 不会刷新整个页面，而是更新页面里的相关部分，从而不打断用户正在做的事情

### 5. 完整例子
```js
let request = new XMLHttpRequest()
request.onreadystatechange = () => {
    if(request.readyState === 400 && request.status === 200){
        console.log(request.responseText)
    }else{
        console.error(request.statusText)
    }
}
request.onerror = (e) => {
    console.error(request.statusText)
}
request.open('GET', 'https://www.example.com/users.json', true)
request.send()
```

## 三、一个请求的一生
`XMLHttpRequest.readyState` 返回一个整数，表示实例对象的当前状态，可能的值有：
> 0，表示实例已经生成，但是实例的 `open` 方法还没有被调用<br>
> 1，表示 `open` 方法已经调用，但是实例的 `send` 方法还没有被调用，仍可以使用实例的 `setRequestHeader` 方法设置 HTTP 请求头信息<br>
> 2，表示实例的 `send` 方法已经被调用，并且服务器返回的头信息和状态码已接收到<br>
> 3，表示正在接收服务器传过来的数据，此时，如果实例的 `responseType` 属性等于 `text` 或空字符串，`responseText` 属性就会包含已经收到的部分信息<br>
> 4，表示服务器返回的数据已经成功完全接收，或本次接收失败
- 通信过程中，每当实例对象发生变化，它的 `readyState` 属性值就会发生变化
- 这个值每一次变化，都会触发 `readystatechange` 事件

## 四、实践
### 1. 准备工作
- 既然需要发请求，就需要一个东西来接收请求啊，服务器 [server.js](https://github.com/xuhaooo/ajax-demo-1/blob/main/server.js)
- 在启动好整个项目之后，发现项目目录里面已经有了一些文件
- 其中 index.html/main.js 这两个文件分别用于写发送请求的 JS 以及展示效果的首页
### 2. 开始实践
#### 2.1 加载 CSS
- 在 main.js 里面通过发 AJAX 请求的方式拿到了服务器返回的 CSS 内容
- 然后通过动态创建 style 标签的方式，在首页展示出来效果
```js
getCSS.onclick = () => {
    const request = new XMLHttpRequest()
    request.open('GET', '/style.css')
    request.onreadystatechange = () => {
        if(request.readyState === 4) {
            if(request.status >= 200 && request.status < 300) {
                const style = document.createElement('style')
                style.innerHTML = request.response
                document.head.appendChild(style)
            }
        }
    }
    request.send()
}
```

### 2.2 加载 JS
- 发送 AJAX 请求拿到服务器返回的 JS 内容
- 通过动态创建 script 标签的方式在控制台打印出了响应的 JS 代码
```js
getJS.onclick = () => {
    const request = new XMLHttpRequest()
    request.open('GET', '/2.js')
    request.onreadystatechange = () => {
        if(request.readyState === 4) {
            if(request.status >= 200 && request.status < 300) {
                const script = document.createElement('script')
                script.innerHTML = request.response
                document.body.appendChild(script)
            }
        }
    }
    request.send()
}
```

### 2.3 加载 HTML
- 动态创建 HTML 标签展示在首页
```js
getHTML.onclick = () => {
    const request = new XMLHttpRequest()
    request.open('GET', '/3.html')
    request.onreadystatechange = () => {
        if(request.readyState === 4) {
            if(request.status >= 200 && request.status < 300) {
                const div = document.createElement('div')
                div.innerHTML = request.response
                document.body.appendChild(div)
            }
        }
    }
    request.send()
}
```

### 2.4 加载 XML
- 控制台打印出 xml 标签里的文本
```js
getXML.onclick = () => {
    const request = new XMLHttpRequest()
    request.open('GET', '/4.xml')
    request.onreadystatechange = () => {
        if(request.readyState === 4) {
            if(request.status >= 200 && request.status < 300) {
                const dom = request.responseXML
                const text = dom.getElementsByTagName('warning')[0].textContent
                log(text.trim())
            }
        }
    }
    request.send()
}
```

### 2.5 加载 JSON
- 控制台打印出 JSON 字符串
```js
getJSON.onclick = () => {
    const request = new XMLHttpRequest()
    request.open('GET', '/5.json')
    request.onreadystatechange = () => {
        if(request.readyState === 4) {
            if(request.status >= 200 && request.status < 300) {
                const object = JSON.parse(request.response)
                myName.textContent = object.name
            }
        }
    }
    request.send()
}
```

### 2.6 加载分页
- 首页写死，后续页通过动态创建 HTML 标签的方式追加在第一页之后
```js
let n = 1
getPage.onclick = () => {
    const request = new XMLHttpRequest()
    request.open('GET', `/page${n+1}`)
    request.onreadystatechange = () => {
        if(request.readyState === 4) {
            if(request.status >= 200 && request.status < 300) {
                const array = JSON.parse(request.response)
                array.forEach(item => {
                    const li = document.createElement('li')
                    li.textContent = item.id
                    xxx.appendChild(li)
                });
                n += 1
            }
        }
    }
    request.send()
}
```

## 五、JSON
### 1. JSON 格式
> JSON 格式（JavaScript Object Notation）是一种用于数据交换的文本格式，2001年由 Douglas Crockford 提出，目的是取代繁琐笨重的 XML 格式
- JSON 对值的类型和格式有严格的规定
> 1. 复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象<br>
> 2. 原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和null（不能使用NaN, Infinity, -Infinity和undefined）<br>
> 3. 字符串必须使用双引号表示，不能使用单引号<br>
> 4. 对象的键名必须放在双引号里面<br>
> 5. 数组或对象最后一个成员的后面，不能加逗号
- 相较于 JS 的七种数据类型，JSON 支持六种数据类型：string、number、bool、null、object、array，不过有上述限制

### 2. JSON 对象
JSON对象是 JavaScript 的原生对象，用来处理 JSON 格式数据。它有两个静态方法：
- JSON.stringify()，将一个值转为 JSON 字符串
- JSON.parse()，将 JSON 字符串转换成对应的值，如果传入的字符串不是有效的 JSON 格式，会将报错
- 为了处理解析错误，可以将 JSON.parse 方法放在 try...catch 代码块中
