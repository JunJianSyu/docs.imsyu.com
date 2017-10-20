> Author: JunJianSyu <br />
> Email: im_syu@163.com
> Description: web 开发面试题回答示例

# 前端工作面试问题

本文包含了一些用于考查候选者的前端面试问题。不建议对单个候选者问及每个问题 (那需要好几个小时)。只要从列表里挑选一些，就能帮助你考查候选者是否具备所需要的技能。

**备注：** 这些问题中很多都是开放性的，可以引发有趣的讨论。这比直接的答案更能体现此人的能力。

## <a name='toc'>目录</a>

  1. [常见问题](#general-questions)
  1. [HTML 相关问题](#html-questions)
  1. [CSS 相关问题](#css-questions)
  1. [JS 相关问题](#js-questions)
  1. [测试相关问题](#testing-questions)
  1. [效能相关问题](#performance-questions)
  1. [网络相关问题](#network-questions)
  1. [代码相关问题](#coding-questions)
  1. [趣味问题](#fun-questions)

## 参与协作

  1. [贡献者](#contributors)
  1. [如何参与贡献](https://github.com/h5bp/Front-end-Developer-Interview-Questions/blob/master/CONTRIBUTING.md)
  1. [许可协议](https://github.com/h5bp/Front-end-Developer-Interview-Questions/blob/master/LICENSE.md)

#### <a name='general-questions'>常见问题：</a>

* 你在昨天/本周学到了什么？

最近在看Nodejs的书, 了解后端编程需要注意的点, 以及Node的一些底层的知识。

* 编写代码的哪些方面能够使你兴奋或感兴趣？

探索新知识,用新知识完成需求。项目完成后的成就感,上线后用户量的增长。

* 你最近遇到过什么技术挑战？你是如何解决的？

在技术挑战上, 如果遇到一些自己无法解决的问题,会去 stack overflow 去寻找解决方案, 或者咨询一些有经验的大牛

* 在制作一个网页应用或网站的过程中，你是如何考虑其 UI、安全性、高性能、SEO、可维护性以及技术因素的？

Ui: 提现在规范上以及设计图表达的出应有的意思,一个好的设计图是有灵魂的 是能吸引用户量的
安全性: XSS跨站脚本攻击、CSRF跨站请求伪造攻击、网络劫持攻击 避免这些问题
高性能: 图片压缩、模块异步加载、图片懒加载、css文件放头部js放尾部、代码进行合并压缩、缩小cookie体积
SEO: html标签语义化属性完整,多页面多链接
构建可维护性web服务前端方面注意知识点: 模块化、命名空间、抽象化、定位问题快速化

* 请谈谈你喜欢的开发环境。

工作环境: 项目有前景, 团队技术过硬, 有想法及技术大牛Leader
硬件: Mac OS

* 你最熟悉哪一套版本控制系统？

Git最为熟悉 Svn用过

* 你能描述当你制作一个网页的工作流程吗？

ui设计稿 -> css/html完成页面布局 -> js完成网页交互需求 -> 代码优化合并压缩 -> 提交版本控制库 -> 部署测试环境 -> 测试 -> 部署上线

* 假若你有 5 个不同的样式文件 (stylesheets), 整合进网站的最好方式是?

合并引入减少请求数

* 你能描述渐进增强 (progressive enhancement) 和优雅降级 (graceful degradation) 之间的不同吗?

渐进增强: 完成网页布局和基本功能, 针对高级浏览器进行效果、交互、追加功能达到更好的体验
优雅降级: 使用新特性完成网页布局和基本功能, 然后对低级浏览器进行hack保证可访问性

* 你如何对网站的文件和资源进行优化？

文件压缩 资源合并 减少请求数

* 浏览器同一时间可以从一个域名下载多少资源？

所有浏览器的并发数量都在10个以下


* 请说出三种减少页面加载时间的方法。(加载时间指感知的时间或者实际加载时间)

减少http请求数、服务器启用gzip、静态资源的体积减小、网址后面加 '/'

* 如果你参与到一个项目中，发现他们使用 Tab 来缩进代码，但是你喜欢空格，你会怎么做？

最好的方法是统一规范: Overview、 JSLint 等等


* 如果今年你打算熟练掌握一项新技术，那会是什么？

机器深度学习

* 请谈谈你对网页标准和标准制定机构重要性的理解。

网页标准化体现在: 结构、表现、行为

* 什么是 FOUC (无样式内容闪烁)？你如何来避免 FOUC？

FOUC: 文档样式短暂失效
造成FOUC主要是: import css引入、css放在页面底部、多个css文件放在html不同的位置

* 请解释什么是 ARIA 和屏幕阅读器 (screenreaders)，以及如何使网站实现无障碍访问 (accessible)。

role属性、aria属性、aria状态属性 元素标签添加title 以及对应阅读属性

* 请解释 CSS 动画和 JavaScript 动画的优缺点。

CSS动画可使用3D加速,动画效果更流畅 缺点是 不可控、兼容性差
JS动画requireAnimationFrame出来之后对渲染帧数进行的优化,动画流畅 动画行为多,可控制性 缺点是 线程出现阻塞影响交互

* 什么是跨域资源共享 (CORS)？它用于解决什么问题？

CORS 跨站资源共享, 允许用户请求不同源的资源, 需要浏览器和服务器同时支持


#### <a name='html-questions'>HTML 相关问题：</a>

* `doctype`(文档类型) 的作用是什么？

doctype 定义文档类型 告诉浏览器该文档已定义类型解析

* 浏览器标准模式 (standards mode) 、几乎标准模式（almost standards mode）和怪异模式 (quirks mode) 之间的区别是什么？

标准模式是已W3C定义的标准解析html  怪异模式是用浏览器厂商的标准解析html

* HTML 和 XHTML 有什么区别？

XHTML是基于XML标准的格式 XHTML很严格 元素标签要正确的嵌套, 元素标签必须闭合。 HTML是基于web网页创建的相对比起来比较随意

* 如果页面使用 'application/xhtml+xml' 会有什么问题吗？

请求服务器的时候告知当前客户端支持 xhtml+xml 格式的文档模式, 返回的xhtml必须严格 标签元素的嵌套、闭合等等。

* 如果网页内容需要支持多语言，你会怎么做？

调用 navigator.language 判断返回浏览器内置语言设置,根据内置语言, 加载语言文本渲染html文档
使用i18n库

* 在设计和开发多语言网站时，有哪些问题你必须要考虑？

语言支持种类, 创建对应语言文件

* 使用 `data-` 属性的好处是什么？

data-* 自定义属性 可以在元素标签上储存数据信息

* 如果把 HTML5 看作做一个开放平台，那它的构建模块有哪些？

<nav> <section> <header> <footer> 语义化元素 模块

* 请描述 `cookies`、`sessionStorage` 和 `localStorage` 的区别。

cookie 储存空间只有4k  sessionStorage 、localStorage 有5M
cookie 可设置过期时间,关闭浏览器时失效 每次跟随请求发送给服务器
sessionStorage 本地保存, 仅在当前会话有效,结束会话时清除, 仅在本地保存不与服务器通讯
localStorage 本地保存, 除非被清除否则永久保存, 仅在本地保存不与服务器通讯

* 请解释 `<script>`、`<script async>` 和 `<script defer>` 的区别。

script 正常按照文档顺序加载外部js文件
async 立即加载外部js文件
defer 延迟加载js文件,等待文档加载完毕之后在加载

* 为什么通常推荐将 CSS `<link>` 放置在 `<head></head>` 之间，而将 JS `<script>` 放置在 `</body>` 之前？你知道有哪些例外吗？

link 放在头部是为了防止FOUC, 样式布局文件也要先于body标签前利于渲染页面
Js放在body后面是利于 dom 加载完毕后利于js进行交互操作, 第二是为了让js请求不会影响页面渲染

* 什么是渐进式渲染 (progressive rendering)？

图片分线性渲染, 渐进式渲染  渐进式就是图片先是一个模糊状态 然后加载完毕之后显示显示清楚



* 你用过哪些不同的 HTML 模板语言？

#### <a name='css-questions'>CSS 相关问题：</a>

* CSS 中类 (classes) 和 ID 的区别。

id是页面唯一的, class可用于多个元素

* 请问 "resetting" 和 "normalizing" CSS 之间的区别？你会如何选择，为什么？

reset.css 清楚浏览器默认一切样式, 统一按照reset样式来进行, 跨浏览器统一
normalizing.css 保留一些浏览器样式

我当然选择 reset 啊! 因为默认样式我重写一次 可以统一样式。


* 请解释浮动 (Floats) 及其工作原理。

float 会使元素块级化, 根据设置的方向脱离文档流浮动到一侧, 普通文档流中的块级元素会当浮动元素不存在

* 描述`z-index`和叠加上下文是如何形成的。

使用定位属性 position 定位元素 z-index 操作元素 Z 轴 操作元素层叠的层次

* 请描述 BFC(Block Formatting Context) 及其如何工作。

BFC块级格式化上下文

内部的Box会在垂直方向，一个接一个地放置。
Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
BFC的区域不会与float box重叠。
BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
计算BFC的高度时，浮动元素也参与计算

* 列举不同的清除浮动的技巧，并指出它们各自适用的使用场景。

浮动元素后,外层父元素检测不到元素, 无法撑开容器 利用清除浮动触发BFC 让容器自适应高度
overflow: hidden   clear: both

* 请解释 CSS sprites，以及你要如何在页面或网站中实现它。

Css精灵主要用于图片合并 生成图标布局html示例 有工具进行手动方式
自动方式: 打包文件时使用 打包工具的插件进行sprites操作 例如gulp配合gulp-css-spriter


* 你最喜欢的图片替换方法是什么，你如何选择使用。

img ait属性
text-indent
最完美的解决方案是  外层元素 position: relative 内层创建一个元素 position: absolute; 撑满 Content写文本 当图片加载失败时 可显示文字

* 你会如何解决特定浏览器的样式问题？

为特定浏览器写css-hack语句

* 如何为有功能限制的浏览器提供网页？

no-script 标签写入内容告知用户无法操作

* 有哪些的隐藏内容的方法 (如果同时还要保证屏幕阅读器可用呢)？

display: none;
visibility: hidden;
最佳方案 元素设置 .div { display: block; overflow: hidden; width: 0; height: 0;}

* 你用过栅格系统 (grid system) 吗？如果使用过，你最喜欢哪种？

bootstrap

* 你用过媒体查询，或针对移动端的布局/CSS 吗？

media query 针对320屏幕做过样式处理


* 你熟悉 SVG 样式的书写吗？

svg是采用xml格式标准类绘制图形, 利用特定属性来绘制样式和填充图形

* 如何优化网页的打印样式？

通过媒体查询引入print.css pt做为单位 打印样式中不要用背景图 不用px做为单位 少用浮动

* 在书写高效 CSS 时会有哪些问题需要考虑？

选择器查找范围缩小 css权重不滥用 依靠css继承属性

* 使用 CSS 预处理器的优缺点有哪些？

css预处理器 优点是 css书写方式变的非常有层次 能使用变量、函数 避免了css冗余代码,代码复用问题 缺点是 维护成本, 样式调试困难

* 如果设计中使用了非标准的字体，你该如何去实现？

@font-face 加载字体 | 或者用图片

* 请解释浏览器是如何判断元素是否匹配某个 CSS 选择器？

带有css选择器匹配的元素拧出来 组成集合 然后从后往前推
例子:
body #div > .span
找出.span 所有的元素 然后从后往前匹配 父级是否为#div 如果不是 从集合中删除 冒泡机制查询

* 请描述伪元素 (pseudo-elements) 及其用途。

为元素标签提供辅助性的布局样式

* 请解释你对盒模型的理解，以及如何在 CSS 中告诉浏览器使用不同的盒模型来渲染你的布局。

盒模型分w3c标准盒子 和 ie盒子 区分在border 通过 box-sizing: border-box 告知浏览器

* 请解释 ```* { box-sizing: border-box; }``` 的作用, 并且说明使用它有什么好处？

统一了盒模型的标准 盒模型中border算入进去

* 请罗列出你所知道的 display 属性的全部值

none、inline、inline-block、table、grid、flex

* 请解释 inline 和 inline-block 的区别？

inline: 定义元素为行内元素
inline-block: 定义元素为行内块元素

* 请解释 relative、fixed、absolute 和 static 元素的区别

relative: 相对定位 保留自身原有位置 属于常规流
fixed: 相对浏览器视口 脱离文档流
absolute: 绝对定位 脱离文档流
static: 常规流

* CSS 中字母 'C' 的意思是叠层 (Cascading)。请问在确定样式的过程中优先级是如何决定的 (请举例)？如何有效使用此系统？

后者覆盖前者 根据权重层叠

* 你在开发或生产环境中使用过哪些 CSS 框架？你觉得应该如何改善他们？

bootstrap 觉得太重 需要自己定制需要的内容

* 请问你有尝试过 CSS Flexbox 或者 Grid 标准规格吗？
* 为什么响应式设计 (responsive design) 和自适应设计 (adaptive design) 不同？
* 你有兼容 retina 屏幕的经历吗？如果有，在什么地方使用了何种技术？
* 请问为何要使用 `translate()` 而非 *absolute positioning*，或反之的理由？为什么？

#### <a name='js-questions'>JS 相关问题：</a>

* 请解释事件代理 (event delegation)。
* 请解释 JavaScript 中 `this` 是如何工作的。
* 请解释原型继承 (prototypal inheritance) 的原理。
* 你怎么看 AMD vs. CommonJS？
* 请解释为什么接下来这段代码不是 IIFE (立即调用的函数表达式)：`function foo(){ }();`.
* 要做哪些改动使它变成 IIFE?
* 描述以下变量的区别：`null`，`undefined` 或 `undeclared`？
* 该如何检测它们？
* 什么是闭包 (closure)，如何使用它，为什么要使用它？
* 请举出一个匿名函数的典型用例？
* 你是如何组织自己的代码？是使用模块模式，还是使用经典继承的方法？
* 请指出 JavaScript 宿主对象 (host objects) 和原生对象 (native objects) 的区别？
* 请指出以下代码的区别：`function Person(){}`、`var person = Person()`、`var person = new Person()`？
* `.call` 和 `.apply` 的区别是什么？
* 请解释 `Function.prototype.bind`？
* 在什么时候你会使用 `document.write()`？
* 请指出浏览器特性检测，特性推断和浏览器 UA 字符串嗅探的区别？
* 请尽可能详尽的解释 Ajax 的工作原理。
* 使用 Ajax 都有哪些优劣？
* 请解释 JSONP 的工作原理，以及它为什么不是真正的 Ajax。
* 你使用过 JavaScript 模板系统吗？
  * 如有使用过，请谈谈你都使用过哪些库？
* 请解释变量声明提升 (hoisting)。
* 请描述事件冒泡机制 (event bubbling)。
* "attribute" 和 "property" 的区别是什么？
* 为什么扩展 JavaScript 内置对象不是好的做法？
* 请指出 document load 和 document DOMContentLoaded 两个事件的区别。
* `==` 和 `===` 有什么不同？
* 请解释 JavaScript 的同源策略 (same-origin policy)。
* 如何实现下列代码：
```javascript
[1,2,3,4,5].duplicator(); // [1,2,3,4,5,1,2,3,4,5]
```
* 什么是三元表达式 (Ternary expression)？“三元 (Ternary)” 表示什么意思？
* 什么是 `"use strict";` ? 使用它的好处和坏处分别是什么？
* 请实现一个遍历至 `100` 的 for loop 循环，在能被 `3` 整除时输出 **"fizz"**，在能被 `5` 整除时输出 **"buzz"**，在能同时被 `3` 和 `5` 整除时输出 **"fizzbuzz"**。
* 为何通常会认为保留网站现有的全局作用域 (global scope) 不去改变它，是较好的选择？
* 为何你会使用 `load` 之类的事件 (event)？此事件有缺点吗？你是否知道其他替代品，以及为何使用它们？
* 请解释什么是单页应用 (single page app), 以及如何使其对搜索引擎友好 (SEO-friendly)。
* 你使用过 Promises 及其 polyfills 吗? 请写出 Promise 的基本用法（ES6）。
* 使用 Promises 而非回调 (callbacks) 优缺点是什么？
* 使用一种可以编译成 JavaScript 的语言来写 JavaScript 代码有哪些优缺点？
* 你使用哪些工具和技术来调试 JavaScript 代码？
* 你会使用怎样的语言结构来遍历对象属性 (object properties) 和数组内容？
* 请解释可变 (mutable) 和不变 (immutable) 对象的区别。
  * 请举出 JavaScript 中一个不变性对象 (immutable object) 的例子？
  * 不变性 (immutability) 有哪些优缺点？
  * 如何用你自己的代码来实现不变性 (immutability)？
* 请解释同步 (synchronous) 和异步 (asynchronous) 函数的区别。
* 什么是事件循环 (event loop)？
  * 请问调用栈 (call stack) 和任务队列 (task queue) 的区别是什么？
* 解释 `function foo() {}` 与 `var foo = function() {}` 用法的区别

#### <a name='testing-questions'>测试相关问题：</a>

* 对代码进行测试的有什么优缺点？
* 你会用什么工具测试你的代码功能？
* 单元测试与功能/集成测试的区别是什么？
* 代码风格 linting 工具的作用是什么？

#### <a name='performance-questions'>效能相关问题：</a>

* 你会用什么工具来查找代码中的性能问题？
* 你会用什么方式来增强网站的页面滚动效能？
* 请解释 layout、painting 和 compositing 的区别。

#### <a name='network-questions'>网络相关问题：</a>

* 为什么传统上利用多个域名来提供网站资源会更有效？
* 请尽可能完整得描述从输入 URL 到整个网页加载完毕及显示在屏幕上的整个流程。
* Long-Polling、Websockets 和 Server-Sent Event 之间有什么区别？
* 请描述以下 request 和 response headers：
  * Diff. between Expires, Date, Age and If-Modified-...
  * Do Not Track
  * Cache-Control
  * Transfer-Encoding
  * ETag
  * X-Frame-Options
* 什么是 HTTP method？请罗列出你所知道的所有 HTTP method，并给出解释。
* 请解释 HTTP status 301 与 302 的区别？

#### <a name='coding-questions'>代码相关的问题：</a>

*问题：`foo`的值是什么？*
```javascript
var foo = 10 + '20';
```

*问题：如何实现以下函数？*
```javascript
add(2, 5); // 7
add(2)(5); // 7
```

*问题：下面的语句的返回值是什么？*
```javascript
"i'm a lasagna hog".split("").reverse().join("");
```

*问题：`window.foo`的值是什么？*
```javascript
( window.foo || ( window.foo = "bar" ) );
```

*问题：下面两个 alert 的结果是什么？*
```javascript
var foo = "Hello";
(function() {
  var bar = " World";
  alert(foo + bar);
})();
alert(foo + bar);
```

*问题：`foo.length`的值是什么？*
```javascript
var foo = [];
foo.push(1);
foo.push(2);
```

*问题：`foo.x`的值是什么？*
```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```

*问题：下面代码的输出是什么？*
```javascript
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
console.log('three');
```

#### <a name='fun-questions'>趣味问题：</a>

* 你最近写过什么的很酷的项目吗？
* 在你使用的开发工具中，最喜欢哪些方面？
* 谁使你踏足了前端开发领域？
* 你有什么业余项目吗？是哪种类型的？
* 你最爱的 IE 特性是什么？
* 你对咖啡有没有什么喜好？


#### <a name='contributors'>贡献者：</a>

本文档始于 2009 年，是以下作者的合作成果：[@paul_irish](https://twitter.com/paul_irish) [@bentruyman](https://twitter.com/bentruyman) [@cowboy](https://twitter.com/cowboy) [@ajpiano](https://twitter.com/ajpiano) [@SlexAxton](https://twitter.com/slexaxton) [@boazsender](https://twitter.com/boazsender) [@miketaylr](https://twitter.com/miketaylr) [@vladikoff](https://twitter.com/vladikoff) [@gf3](https://twitter.com/gf3) [@jon_neal](https://twitter.com/jon_neal) [@sambreed](https://twitter.com/sambreed) 和 [@iansym](https://twitter.com/iansym)。

时至今日，文档已经融入超过 [100 位开发者](https://github.com/h5bp/Front-end-Developer-Interview-Questions/graphs/contributors)的贡献。
