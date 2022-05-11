## HTML 面试题

### 1. src 和 href 的区别

src: 表示对资源的引用，它指向的内容会被嵌入到当前标签所在到位置。当浏览器解析到有 src 的元素时会暂停其他资源的下载和处理，知道该资源加载、编译、执行完毕。

href: 表示对超文本的引用，它指向的是一些网络资源，建立和当前元素或本文档的链接关系。当浏览器识别到它指向的文件时会并行下载资源，不会停止对当前文档的处理。

一句话：src 会阻塞后续资源的下载和处理，href 不会。

### 2. HTML 语义化的理解

1. html 语义化让页面的内容结构化，使结构更加清晰，便于浏览器、搜索引擎解析
2. 即使在没有 css 情况下页面也可以以一种文档格式显示，并且是容易阅读的
3. 搜索引擎的爬虫也依赖于 HTML 标记来确定上下文和各个关键字的权重，利于 SEO
4. 便于阅读维护理解

我觉得可以从三个方面来进行总结： 1. 面向机器，说白了就是可以让机器更好的读懂页面内容，与之相对应的就是浏览器、搜索引擎的解析，SEO 优化。 2. 面向开发人员，html 语义化可以让页面结构更加清晰明了，便于开发人员的阅读和维护。 3. 面向用户，与之对应的就是 在没有 css 的情况下，对用户的阅读相对友好

### 3. 常用的 meta 标签

meta 标签通常用于指定网页的描述，关键字，以及其他数据信息。

1. charset，用来描述 HTML 文档编码类型

`<meta charset="UTF-8" >`

2. keywords，页面关键词：

`<meta name="keywords" content="关键词" />`

3. description，页面描述：

`<meta name="description" content="页面描述内容" />`

4. viewport，适配移动端，可以控制视口的大小和比例：

`<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">`

### 4. web worker

Web Worker 的作用，就是为 JS 创造多线程环境。允许主线程创建 Worker 线程，将一些任务分配给后者运行。 [阮一峰-Web Worker 使用教程](https://www.ruanyifeng.com/blog/2018/07/web-worker.html)

### 5. HTML5 离线缓存-manifest

manifest 是一个后缀名为 manifest 的文件，在文件中定义那些需要缓存的文件，支持 manifest 的浏览器，会将按照 manifest 文件的规则将文件保存在本地，从而在没有网络的情况下，也能访问页面。

原理：

> HTML5 的离线存储是基于一个新建的 .appcache 文件的缓存机制（不是存储技术），通过这个文件上的解析清单离线存储资源，这些资源就会像 cookie 一样被存储了下来。之后当网络在处于离线状态下时，览器会通过被离线存储的数据进行页面展示。

> 当我们第一次正确配置 app cache 后，当我们再次访问该应用时，浏览器会首先检查 manifest 文件是否有变动，如果有变动就会把相应的变得跟新下来，同时改变浏览器里面的 app cache，如果没有变动，就会直接把 app cache 的资源返回，基本流程是这样的。

如何使用：

1. 创建一个和 html 同名的 manifest 文件，比如页面为 index.html，那么可以建一个 index.manifest 的文件，然后给 index.html 的 html 标签添加如下属性即可：

```html
<html lang="en" manifest="index.manifest"></html>
```

2. 在如下 cache.manifest 文件的编写离线存储的资源。

```test
CACHE MANIFEST
   	#v0.11
   	CACHE:
   	js/app.js
   	css/style.css
   	NETWORK:
   	resourse/logo.png
   	FALLBACK:
   	/ /offline.html
```

> manifest 文件，基本格式为三段： CACHE， NETWORK，与 FALLBACK，其中 NETWORK 和 FALLBACK 为可选项。而第一行 CACHE MANIFEST 为固定格式，必须写在前面。

> CACHE: 表示需要离线存储的资源列表，由于包含 manifest 文件的页面将被自动离线存储，所以不需要把页面自身也列出来。

> NETWORK: 表示在它下面列出来的资源只有在在线的情况下才能访问，他们不会被离线存储，所以在离线情况下无法使用这些资源。不过，如果在 CACHE 和 NETWORK 中有一个相同的资源，那么这个资源还是会被离线存储，也就是说 CACHE 的优先级更高。

> FALLBACK: 表示如果访问第一个资源失败，那么就使用第二个资源来替换他，比如上面这个文件表示的就是如果访问根目录下任何一个资源失败了，那么就去访问 offline.html 。

如何更新缓存：

1. 更新 manifest 文件
2. 通过 js 操作，html5 中引入了 js 操作离线缓存的方法，`window.applicationCache.update();`
3. 清楚浏览器缓存

注意事项：

1. 浏览器对缓存数据的容量限制可能不太一样（某些浏览器设置的限制是每个站点 5MB）。
1. 如果 manifest 文件，或者内部列举的某一个文件不能正常下载，整个更新过程都将失败，浏览器继续全部使用老的缓存。
1. 引用 manifest 的 html 必须与 manifest 文件同源，在同一个域下。
1. FALLBACK 中的资源必须和 manifest 文件同源。
1. 当一个资源被缓存后，该浏览器直接请求这个绝对路径也会访问缓存中的资源。
1. 站点中的其他页面即使没有设置 manifest 属性，请求的资源如果在缓存中也从缓存中访问。
1. 当 manifest 文件发生改变时，资源请求本身也会触发更新。

更详细的可以参考：[HTML5 离线缓存-manifest 简介](https://yanhaijing.com/html/2014/12/28/html5-manifest/)

### 6. 请描述一下 cookies，sessionStorage 和 localStorage 的区别？

cookie: 其实最开始是服务器端用于记录用户状态的一种方式，由服务器设置，在客户端存储，然后每次发起同源请求时，发送给服务器端。cookie 最多能存储 4 k 数据，它的生存时间由 expires 属性指定，并且 cookie 只能被同源的页面访问共享。

localStorage, sessionStorage: 是 html5 提供的一种浏览器本地存储的方法，一般能够存储 5M 或者更大的数据.

localStorage 和 sessionStorage 的不同: sessionStorage 代表的是一次会话中所保存的数据。在当前窗口关闭后就会消失，并且只能被同一个窗口的同源页面共享。 localStorage 除非手动删除，否则不会消失，并且在所有同源窗口中共享。

参考资料：[请描述一下 cookies，sessionStorage 和 localStorage 的区别？](https://segmentfault.com/a/1190000017423117)

### 7. iframe 有那些优点和缺点？

优点：
- 用来加载速度较慢的内容（如广告）
- 可以使脚本可以并行下载
- 可以实现跨子域通信

缺点：
- iframe 会阻塞主页面的 onload 事件，可以通过动态给 iframe 的 src 赋值来解决
- SEO 不友好
- 浏览器的后退按钮失效
- iframe 和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。

### 8. async 和 defer 的作用是什么？有什么区别？
1. 脚本没有 async 和 defer 时会立即加载冰执行，不会等待后续载入文档元素，读到就加载并执行
1. defer 表示延迟执行引入的 js，js 在加载时 HTML 并不会停止解析，这两个过程是并行的。当整个 document 解析完毕后再执行脚本文件，在 DOMContentLoaded 事件触发之前完成。多个脚本按顺序执行。
1. async 属性表示异步执行引入的 JavaScript，与 defer 的区别在于，如果已经加载好，就会开始执行，也就是说它的执行仍然会阻塞文档的解析，只是它的加载过程不会阻塞。多个脚本的执行顺序无法保证。

### 9. 重绘和回流
回流（Reflow）：当Render Tree中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。

会导致回流的操作：

1. 页面首次渲染
1. 浏览器窗口大小发生改变
1. 元素尺寸或位置发生改变
1. 元素内容变化（文字数量或图片大小等等）
1. 元素字体大小变化
1. 添加或者删除可见的DOM元素
1. 激活CSS伪类（例如：:hover）
1. 查询某些属性或调用某些方法

重绘（Repaint）：当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。

一句话：回流必将引起重绘，重绘不一定会引起回流。而回流比重绘的代价要更高。

简单总结一下：会引起元素位置变化的就会reflow，如上面介绍的，窗口大小改变、字体大小改变、以及元素位置改变，都会引起周围的元素改变他们以前的位置；不会引起位置变化的，只是在以前的位置进行改变背景颜色等，只会repaint；

如何避免：

css:
1. 避免使用 table 布局
1. 尽可能在DOM树的最末端改变class。
1. 避免设置多层内联样式
1. 将动画效果应用到position属性为absolute或fixed的元素上
1. 避免使用CSS表达式（例如：calc()）。

js: 
1. 避免频繁操作样式，最好一次性重写style属性，或者将样式列表定义为class并一次性更改class属性。
1. 避免频繁操作DOM，创建一个documentFragment，在它上面应用所有DOM操作，最后再把它添加到文档中。
1. 也可以先为元素设置display: none，操作结束后再把它显示出来。因为在display属性为none的元素上进行的DOM操作不会引发回流和重绘。
1. 避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来
1. 对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。