## HTML面试手册

### html5新特性

- 用于媒介回放的video元素和audio元素

- 本地离线存储localStorage长期存储数据，浏览器关闭后数据不丢失

- sessionStorage的数据在浏览器关闭后自动删除
- 语义化更好的标签，如article、footer、header、nav、section等
- 设备兼容特性 HTML5提供了前所未有的数据与应用接入开放接口
- 新的技术web worker、websocket、Geolocation，增加拖放API、绘画canvas

### 行内元素、块级元素和空元素

- 行内元素：`span`,`img`,`input`,`select`,`a`,`strong`等
- 块级元素：`div`,`ul`,`li`,`h1-5`,`p`,`dl`,`dt`,`dd`等
- 空元素：`br`,`hr`,`link`,`meta`等

### 网站文件和资源进行优化

- 文件合并（减少http请求）
- 文件压缩（减少文件下载体积）
- 使用缓存
- 反向链接，网站外链优化
- 使用CDN托管资源
- gzip压缩需要的js和css文件
- meta标签优化（`title`, `description`, `keywords`）,`heading`标签的优化,`alt`优化

### 页面导入css，使用link和@import的区别

- `link`是html标签，而`@import`是css的
- `@import`存在兼容问题，只在IE5以上可用，而`link`无兼容问题
- 页面被加载时，`link`会同时被加载，而`@import`引用的css会等到页面被加载完再加载
- `link`的权重高于`@import`的权重

### label的作用

`label`用于定义表单控制间的关系，当用户选择该标签时，浏览器会自动将焦点转到和标签相关的表单控件上。

```html
<label for="Name">Number:</label> 
<input type=“text“ name="Name" id="Name"/>
```

### 标签的title属性和alt属性的区别

- `alt`是为了在图片未能正常显示时给予文字说明。
- `title`属性为设置该属性的元素提供建议性的信息。

### 语义化标签的好处

- 去掉或者丢失样式的时候能让页面呈现出清晰的结构

- 方便设备解析渲染网页

- 有利于`SEO`，与搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重

- 便于团队开发和维护，语义化更具可读性

### 浏览器内多个标签页通信

使用localStorage、cookies等本地存储方式。

### iframe的优缺点

优点：

- 可以跨域通信
- 可以实现无刷新文件上传
- 解决第三方内容的加载问题

缺点：

- 不利于SEO
- 页面会增加服务器请求
- 会阻塞主页面的Onload事件

### src和href区别

- `src`用于替换当前元素；href用于在当前文档和引用资源间确立联系
- `src`指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；而`href`指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接。

### 为何多个域名来存储网站资源会更有效

- 节约`cookie`宽带；
- `CDN`缓存更加方便；
- 突破浏览器并发限制；
- 节约主域名的连接数，优化页面下响应速度；
- 防止不必要的安全问题；

### HTML5本地存储

`HTML5`的`Web storage`的存储方式有：`sessionStorage`和`localStorage`。

- `sessionStorage`用于本地存储一个会话中的数据，当会话结束后就会销毁；

- `localStorage`用于持久化的本地存储，除非用户主动删除数据，否则数据永远不会过期；

- `cookie`是网站为了标示用户身份而储存在用户本地上的数据。

区别：

- **从浏览器和服务器间的传递看**： `cookie`数据始终在同源的http请求中携带，即`cookie`在浏览器和服务器间来回传递；而`sessionStorage`和`localStorage`仅在本地保存，不会自动把数据发给服务器。

- **从大小看**： 存储大小限制不同，`cookie`数据不能超过`4k`，只适合保存很小的数据；而`sessionStorage`和`localStorage` 也有存储大小的限制，但比`cookie`大得多，可以达到5M或更大。

- **从数据有效期看**： `sessionStorage`在会话关闭会立刻关闭，因此持续性不久；`cookie`在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。而`localStorage`始终有效。

- **从作用域看**： `sessionStorage`不在不同的浏览器窗口中共享，即使是同一个页面；而`localStorage`和`cookie`都是可以在所有的同源窗口中共享的。

### 常见的浏览器内核

| 浏览器/RunTime | 内核（渲染引擎） | JavaScript引擎                                               |
| -------------- | ---------------- | ------------------------------------------------------------ |
| Chrome         | webkit->blink    | V8                                                           |
| FireFox        | Gecko            | SpiderMonkey                                                 |
| Safari         | Webkit           | JavaScriptCore                                               |
| Edge           | EdgeHTML         | Chakra(for JavaScript)                                       |
| IE             | Trident          | JScript(IE3.0-IE8.0)                                         |
| Opera          | Persto->blink    | Linear A（4.0-6.1）/ Linear B（7.0-9.2）/ Futhark（9.5-10.2）/ Carakan（10.5-） |
| Node.js        | -                | V8                                                           |

****

如果本文对您有帮助，请帮忙`点赞`支持哇～

