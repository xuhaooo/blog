# 历史
简要回顾一下 WWW 的历史，大概 30 年前李爵士发明了 WWW （World Wide Web），一种适用于全世界的网络。其主要包含 3 个概念：
- URI（包含 URN、URL），俗称网址
- HTTP，两个电脑之间传输内容的协议
- HTML，主要用来做页面跳转

> URL 的作用是能让你访问一个页面；
HTTP 的作用是让你能下载这个页面（即你用这个协议，服务器就知道怎么把你访问的页面传给你）；
HTML 的作用是让页面变成你能看懂的样子；
于是一个简单而完美的系统就此形成。

当然，除了概念他还做出了实践：
>发明了第一个服务器
发明了第一个浏览器
写出了第一个网页

# URI 是什么
>URI（统一资源标识符）分为 URN 和 URL
>一般使用 URL 做网址

>URN（统一资源名称）
>它能唯一确定一个东西，如书的 ISBN 码

>URL（统一资源定位符）
>它能唯一确定一个位置（东西可能会更改，但位置不会变），更实用
>常见的 URL 组成：```https://www.baidu.com/s?wd=hello&rsv_spt=1#5```
>```
>https:// 表示协议
>```
>```
>www.baidu.com 表示域名
>注：www.baidu.com 与 baidu.com 并不是同一个域名
>它们只是共享了同一个二级域名（baidu.com）
>.com 是顶级域名，也称一级域名，baidu.com 是二级域名，但很多人略过 .com 直接叫 baidu.com 为一级域名
>www.baidu.com 是三级域名
>```
>```
>/s 表示路径
>特别注意：路径与文件不是一个东西，不能说明百度服务器中有一个 s 的文件
>```
>```
>?wd=hello&rsv_spt=1 表示查询字符串（查询参数）
>```
>```
>#5 表示锚点
>```

# DNS（Domain Name System） 域名系统
>**给它一个域名还你一个 IP**
比如：运行 ```nslookup baidu.com``` 或 ```ping baidu.com```即可得到 DNS 返回的离你最近的百度服务器的 IP
在浏览器输入域名，浏览器会去问路由对应的 IP 是多少，路由就会去问电信，而电信是有责任维护域名与 IP 的对应关系的，它帮你解析域名，综上帮你解析域名对应 IP 的服务器就是 DNS

>**自己指定 IP**
既然我们输入域名得通过 DNS 服务器解析 IP 才能成功访问一个网站，那我们能不能不通过 DNS，自己指定 IP 访问呢？
> - Windows用户： 自己谷歌 windows 设置 hosts 方法
> - 非 Windows 用户：运行 ```sudo vi /etc/hosts```，加一行 ```127.0.0.1 baidu.com```即可设置 baidui.com 的 IP 为 127.0.0.1

>**这种方法可以绕过 DNS 服务器自己指定 IP**
**用处**：必要时期，可以自己设置谷歌 IP 来访问谷歌
**例如**：某一天，你输入 google.com 想访问谷歌，然后 DNS 会返回一个假的 IP 给你，至于为什么是假的，你猜。
    那么这时怎么办呢，拿不到谷歌的 IP 我们就永远访问不了谷歌，假如谷歌的 IP 是 1.2.3.4，我们就可以自己设置 google.com 对应的 IP 为 1.2.3.4，这样就可以访问啦，不需要电信 DNS 的 IP。但是这个方法不靠谱，因为谷歌的 IP 经常会变，一变就失效了，只能临时使用。

# HTTP
HTTP（HyperText Transfer Protocol）是超文本传输协议，客户端、服务器根据它的规定进行请求和响应，相当于密码本

# 请求与响应
## 一、Sever + Client + HTTP 
![](https://upload-images.jianshu.io/upload_images/1020017-625bae64555b98bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 浏览器负责发送请求
- 服务器在80端口接收请求
```端口主要与 TCP/IP 协议有关，0-2048 每个端口会被指定做什么，比如：21专门做ftp服务；80专门做HTTP服务```
- 服务器负责返回内容（响应）
- 浏览器负责下载响应内容
- HTTP 的作用就是指导浏览器和服务器如何沟通
```比如：发送域名时你把域名打错了，服务器就响应 404 错误；服务器没开机或坏掉了，那么服务器就响应 502 错误```

## 二、请求
### 请求实例与分类
#### 1. GET 请求
```
curl -s -v -H "Tom: xxx" -- http://www.baidu.com
-s（--slient）: 不要显示进度条
-v: 要显示请求和响应，没有 -v 就只显示请求
-H "Tom: xxx": 添加一个请求头
```
> 用 curl 创造了一个请求并得到了响应：

![GET 请求](https://upload-images.jianshu.io/upload_images/1020017-d9de993445932dcd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


‘>’后面的是请求内容；‘<’后面的是响应内容
> 向百度服务器发送的请求内容为：
GET / HTTP/1.1 ：获取 根目录 协议1.1版本
Host: www.baidu.com ：把我想要访问的域名告诉他
User-Agent: curl/7.54.0 ：我用什么软件发的请求
Accept: */* ：接收你返回的任何内容
Tom: xxx ： 请求头 可以不加

#### 2. POST 请求
```
curl -X POST -s -v -H "Tom: xxx" -- "https://www.baidu.com"
```
> 请求的内容为：
POST / HTTP/1.1
Host: www.baidu.com
User-Agent: curl/7.54.0
Accept: */*
Tom: xxx

#### 3. POST 带数据的请求
```
curl -X POST -d "1234567890" -s -v -H "Tom: xxx" -- "https://www.baidu.com"
```
> 请求的内容为：
>POST / HTTP/1.1
>Host: www.baidu.com
>User-Agent: curl/7.45.0
>Accept: */*
>Tom: xxx
>Content-Length: 10 ：上传 10 个字节
>Content-Type: application/x-www-form-urlencoded
>
>1234567890

#### 4. 总结
- GET 请求、POST 请求、POST 带数据的请求
- 其实 GET 也可以上传数据，但一般没人这么干，因为服务器不接受，它觉得你来拿东西不会还送点东西
- ```curl -X POST -s -v -H "Host: www.qq.com" -- "https://www.baidu.com"```
你可以这么做，但是访问的是百度的服务器，然后问有没有 qq 的页面，当然返回错误咯

### 请求的格式
>1 动词 路径 协议/版本
2 key1: value1
2 key2: value2
2 key3: value3
2 Content-Type: application/x-www-form-urlencoded
2 Host: www.baidu.com
> 2 User-Agent: curl/7.45.0
> 3
> 4 要上传的数据
- 请求最多包含 4 部分，最少包含 3 部分（也就是说第四部分可以为空）
- 第三部分永远是一个回车
- GET POST PUT（整体更新）PATCH（局部更新）DELETE
- 这里的路径包含**查询参数**，但不包含**锚点**，服务器不看锚点，锚点是浏览器看的
- 如果你没有写路径，路径默认为 /
- 请求里的路径只是路径而已，跟文件没有关系，文件格式只和 Accept 里写的有关系
- 第 2 部分中的 Content-Type 标注了第 4 部分的格式：
    ```Content-Type: application/x-www-form-urlencoded```

## 三、响应
请求之后应该都能得到一个响应，除非断网或服务器宕机了
### 响应实例与分类
#### 1. 响应 GET 请求
>HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 2443
Content-Type: text/html
>
><!DOCTYPE html>
<!--STATUS OK--><html><head>...

请求时我们会带上我们的数据，百度响应时也会带上百度的数据
第四部分服务器返回的是字符串

#### 2. 响应 POST 请求
>HTTP/1.1 401 Unauthorized
Connection: Keep-Alive
Content-Length: 17922
Content-Type: text/html
>
><!DOCTYPE html>
<!--STATUS OK--><html><head>...

登录请求时输入的密码错了，服务器返回 401 错误

#### 3. 注：
- GET 请求和 POST 请求对应的响应可以一样，也可以不一样
- 响应的第四部分可以很长

### 响应的格式
> 1 协议/版本号 状态码 状态解释
2 key1: value1
2 key2: valuw2
2 Content-Length: 21124
2 Content-Type: text/html
3
4 要下载的内容

### 状态码
- 1xx 不常用 用于指定客户端应响应的某些动作
- 2xx 表示成功
    >200 通常的成功 OK
    >204 成功处理请求，但不会刷新页面
    >>表示服务器接收到的请求已经处理完毕，但浏览器页面不会刷新
- 3xx 表示你（访问者）可以走了
    >301 永久重定向 Moved Permanently
    >>请求的网页已经永久移动到新位置，浏览器会转到另一个网址，永久性重定向与临时重定向不同的是，浏览器有可能会缓存，记住结果
    
    >302 Found 临时重定向
    >>HTTP/1.0就有的，POST 方法的重定向在未问询用户的情况下就会变成 GET
    
    >303 和 307
    >>这两个是 HTTP/1.1新加的，都是临时重定向，303 与 302 一样，POST 重定向为 GET，307 不同的是把 POST 转为 GET 的
    
    >304 Not Modified
    >>资源未被修改过，还是返回跟上次一样的东西
- 4xx 表示你（访问者）错了
    >400 Bad Request
    >>客户端中存在语法错误
    
    >401 Unauthorized
    >>用户未授权，需要用户验证
    
    >403 Forbidden
    >>服务器已经理解请求，但拒绝执行
    
    >404 Not Found
    >>服务器找不到请求网页
- 5xx 表示我（服务器）错了
    >500 Internal Server Error
    >>服务器遇到错误，无法完成请求
    
    >503 Service Unavailable
    >>由于临时的服务器维护或者过载，暂时无法处理请求

## 四、用 Chrome 查看请求和响应
> 1. 打开 Network
> 2. 输入网址
> 3. 查看请求和响应
>>- 在 Request Headers 里点击 view source 查看请求；如果请求有第 4 部分则在 FormData 或 Payload 里可以看到
>>- 在 Response Headers 里点击 view source 查看响应；如果响应有第 4 部分则在 Response 或 Preview 里可以看到