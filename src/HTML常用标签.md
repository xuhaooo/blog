## a 标签
### 1. 属性
#### href
跳转的地址；h-hyper; ref-reference


#### target
指定在哪个窗口打开超链接

#### download
点击下载网页；很多浏览器不支持，一般不用

#### rel
`rel="noopener"`
防止一个 bug，学了 JS 再说

### 2. 作用
1. 跳转外部页面
2. 跳转内部锚点
3. 跳转到邮箱或电话等


### 3. a 的 href 的取值
#### 网址
- https://google.com
- http://google.com
- //google.com 无协议网址<br>
无协议网址会根据当前的页面的协议，进行跳转

#### 路径
- 绝对路径，/a/b/c<br>
当加上了 http:// 之后，最前面的 / 就不再是相对文件来说的根目录，而是把当前页面所在的目录当做根目录

- 相对路径，a/b/c<br>
相对于当前目录，./a/b/c

- 伪协议<br>
`<a href="javascript:;"></a>`
作用就是可以写出一个点击什么都不做的超链接

- 页面内跳转锚点<br>
点击跳转到页面中对应锚点位置
```
<a href="#id"></a>
<h2 id="id"></h2>
```

### 4. a 的 target 的取值
#### 内置名字
_blank：在新页面打开<br>
_self：默认值，在当前页面打开<br>
_top：结合 iframe 标签，在顶层窗口打开<br>
_parent：结合 iframe 标签，在父级窗口打开

#### 程序员命名
比如 target="xxx"，意思是如果有个窗口叫 xxx，那么就在该窗口打开，如果没有那就创建一个叫 xxx 的窗口，然后在里面打开


## table 标签
- 一个完整的表格目标
```
<table>
    <thead>
        <tr>
            <th></th>
        <tr>
    </thead>
    <tbody>
        <tr>
            <td></td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td></td>
        </tr>
    </tfoot>
</table>
```

- 其他属性
```
/* 自动调整表格单元格大小 */
table-layout: auto/fixed;
/* 边框之间的距离 */
border-spacing: 0;
/* 边框合并 */
border-collapse: collapse;
```

## img 标签
### 作用
发出 get 请求，展示一张图片

### 属性
alt/height/width/src<br>
替换文字、宽高、图片路径<br>
永远不要让图片变形，宽高只能给一个<br>

### 事件
onload/onerror，用 JS 补救图片加载失败

### 响应式
`img {max-width: 100%;}`
一行 CSS，让图片适应手机屏幕

### 可替换元素
iframe/video/embed


## form 标签
### 作用
发 get 或 post 请求，然后刷新页面

### 属性
action： 请求路径<br>
method：请求方式，不写默认 POST<br>
autocomplete：on/off，自动填充，会给予推荐选项<br>
target：用哪个页面展示请求的页面

### 事件（onsubmit）
form 必须要有一个 type="submit" 的按钮

## input 属性
### 作用
让用户输入内容

### type 属性
button/email/number/password/text/search/submit/tel/radio/checkbox/file/hiddem

### 其他属性
name/autofocus/checked/disabled/maxlength/pattern/value/placeholder

### 事件
onchange/onfocus/onblur<br>
鼠标改变、点击、移出

## textarea 标签
用 `resize: none;` 禁止拖动

## select 标签
```
<select>
    <option>--请选择--</option>
    <option>选项一</option>
    <option>选项二</option>
</select>
```
