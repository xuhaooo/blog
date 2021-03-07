# HTML 入门笔记 1
## 谁发明了HTML
> Tim Berners-Lee（李爵士）
> 除此之外，他还发明了世界上第一浏览器、第一个服务器、写出了第一个网页，前端程序员的祖师爷，膜拜。。

## HTML 起手式应该写什么
```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device=width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>网页标题</title>
</head>
<body>
</body>
</html>
```

## 章节标签（表示文章/书的层级）
- 标题（h1-h6）
```
<h1>一级标题</h1>
<h2>二级标题</h2>
```
- 章节（section）
```
<section>
表示一个包含在HTML文档中的独立部分，它没有更具体的语义元素来表示，一般来说会有包含一个标题
</section>
```

- 文章（article）
```
<article>
表示文档、页面、应用或网站中的独立结构
</article>
```

- 段落（p）
```
<p>一个段落</p>
```

- 头部（header）
```
<header>
用于展示介绍性内容，通常包含一组介绍性的或是辅助导航的元素。它可能包含一些标题元素，但也可能包含其他元素，比如 Logo、搜索框、作者名称，等等。
</header>
```

- 脚部（footer）
```
<footer>
表示最近一个章节内容元素的页脚。一个页脚通常包含该章节作者、版权数据或者与文档相关的链接等信息。
</footer>
```

- 主要内容（main）
```
<main>
网页的主体部分
</main>
```

- 旁支内容（aside）
```
<aside>
表示一个和其余页面内容几乎无关的部分
</aside>
```

- 划分（div）
```
<div>
没有任何意义的网页划分元素
</div>
```

## 全局属性
- class
    标记一个元素，可以包含多个值
- style
HTML 属性，注释样式，可以用 JS 设置
HTML、CSS、JS 设置属性，JS 优先级最高
- contenteditable
网页上元素内容可以编辑
- hidden
- id
标记一个网页上唯一的元素
能不要就不用，它的唯一性没有保障，就算有 2 个相同 id，也不报错
- tabindex
控制 tab 的顺序
可以是整数，但不要求连续
值为 0，表示最后才被 tab 访问
值为 -1，表示不可被 tab 访问
- title
设置了值，鼠标移动到该元素上，会显示对应的值（提示内容）

## 内容标签
- ol + li
有序列表
```
<ol>
    <li>列表项</li>
</ol>
```

- ul + li
无序列表
- dl + dt + dd
dl：一个包含术语定义以及描述的列表
dt：描述的关键词
dd：具体描述项
```
<dl>
    <dt></dt>
    <dd></dd>
</dl>
```
- pre
里面内容写成啥样就显示成啥样，不会省略空格、回车、tab
- hr
水平线
- br
强制换行
- a
链接
- em
语气上的强调
- strong
内容本身很重要的强调
- code 
一行显示代码，用 pre 包起来就可多行显示代码
- quote
单行引用
- blockquote
多行引用
