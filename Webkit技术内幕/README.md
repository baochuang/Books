# Webkit
Q：Webkit是什么？  
A： 浏览器的内核 - 渲染引擎（WebCore）。
Q: 讲什么？  
A: 从HTML5基础知识入手，讲述Webkit内部渲染HTML网页的原理，Chromium的实现。  
Q: 目标是什么？  
A: 为Web前端开发做出启示。
## 浏览器和浏览器内核

### 浏览器历史
WorldWideWeb(后改名为Nexus，1990年底) - Mosaic(Netscape 1993) -  IE(1995 第一次浏览器大战 IE VS Netscape) - Safari(2003发布) - Firefox(2004年发布第一版 第二次浏览器大战开始 Firefox VS IE) - Webkit(2005开源Safari内核项目) -  Chrome(2008年， 以Webkit项目创建了Chromium项目，项目本身就是一个浏览器，Chrome选其稳定版作为基础，V8也同时诞生， PC端浏览器大战持续 Firefox VS IE VS Chrome)

### Web应用的发展
HTML第一版(1993) - HTML5(2014年10月28日，W3C推荐标准)  

CSS第一版(1996年12月W3C推出) - CSS3(2001年5月W3C开始定制一直到现在)  

JavaScript(1995年 名字从Mocha到LiveScript再到JavaScript， 1996年三月JavaScript被内置到Navigator 2.0) - ECMAScript第一版(1997年7月发布) - ECMAScript 2015（2015年6月）  

DOM(1997年DHTML发布，DOM模式正式应用)  

XMLHttpRequest(1999年)  - Ajax(2005年)

JSON(2001年)  

2004年Dojo框架诞生，标志着JavaScript编程框架时代来临 - Jquery(2006年)

Node.js(2009年，它标志着JavaScript可以用于服务器端编程，从此网站的前端和后端可以使用同一种语言开发。） - NPM,BackboneJS和RequireJS(2010年，标志着JavaScript进入模块化开发的时代) - AngularJS和Ember(2012年，单页面应用程序框架)开始崛起 - JSON标准国际化(2013年) - React(2013年5月)
### 当前主流浏览器使用内核
* **IE** Trident
* **Safari** Webkit
* **Chrome** Webkit -> Blink(2013)
* **Firefox** Gecko

### HTML5的十大类别和规范
|     类别  |    具体规范    |
|-----------|:-------------:|
| 离线       |  Application cache, local storage, Indexed DB,在线/离线事件                                   |
| 存储       |  Application cache, local storage, Indexed DB等                                              |
| 连接       |  Web Sockets, Server-sent事件                                                                |
| 文件访问   |  File API, File System, FileWriter, ProgressEvents                                           |
| 语义       |  各种新的元素，包括Media,structural,国际化,Link relation,属性, form类型,microdata等方面         |
| 音频和视频 | HTML5 Video, Web Audio, Web RTC, Video track等                                               |
| 3D和图形   | Canvas 2D, 3D CSS变换, Web GL, SVG等                                                         |
| 展示      | CSS3 2D/3D变换，转换(transition), WebFonts等                                                  |
| 性能      | Web Worker, HTTP caching等                                                                   |
| 其他      | 触控和鼠标，Shadow DOM, CSS masking等                                                         |
## HTML网页和结构

### 层次结构
网页的层次结构是指网页中的元素可以分布在不同的层次中。  
对于需要复杂变换和处理的元素，它们需要新层，如canvas, video, 使用css动画部分的div
## Webkit架构和模块
Webkit Ports: 对于不同浏览器使用的Webkit来说，不同平台，依赖的第三方库不同等，都需要移植这一部分去对接调用。 
Content接口和Content模块: 对渲染网页功能的抽象，用来渲染网页内容的模块。
Android WebView: 替换Android系统默认的WebView
CC: 图层渲染架构，支持同步或异步的渲染模式，支持硬件和软件的合成输出模式等。
### Chromium多进程模型
* **Browser进程** 浏览器的主进程，负责浏览器界面的显示，各个页面的管理，负责其他进程的创建和销毁，只有一个
* **Renderer进程** 网页的渲染进程
* **GPU进程** 主要是负责把Renderer进程中绘制好的tile位图作为纹理上传至GPU，并调用GPU的相关方法把纹理draw到屏幕上，最多只有一个，当且仅当GPU硬件加速打开的时候才会被创建。
PS: 在Android版中 GPU成为Browser进程下的一个线程，Renderer进程会成为Android系统下的一个系统进程,被**影子协议**严格限制个数  
## 资源加载和网络栈

## HTML解释器和DOM模型
**Webkit使用预扫描和预加载机制来实现资源的并发下载而不被JavaScript的执行所阻碍** 当需要执行JavaScript代码的时候，Webkit先暂停当前JavaScript代码的执行，使用预先扫描器HTMLPreloadScanner类来扫描后面的词语，如果Webkit发现它们使用其他资源，那么使用预资源加载器HTMLResourcePreloader类来发送请求，在这之后才执行JavaScript的代码。
**当DOM树构建完成后，Webkit触发“DOMContentLoaded"事件，当所有资源都被加载完成后，Webkit触发”onload“事件**
### DOM模型

### HTML解释器

### DOM的事件机制

### Shadow DOM
Shadow-dom 是游离在 DOM 树之外的节点树，但是他的创建基于普通 DOM 元素（非 document），并且创建后的 Shadow-dom 节点可以从界面上直观的看到。更重要的是，Shadow-dom 具有良好的密封性。

这是浏览器提供的一种“封装”功能，提供了一种强大的技术去隐藏一些实现细节。
## CSS解释器和样式布局

### CSS基本功能

### CSS解释器和规则匹配
Webkit使用Flex和Bison解析生成器从CSS语法文件中自动生成解析器。回忆一下解析器的介绍，Bison创建一个自底向上的解析器，Firefox使用自顶向下解析器。它们都是将每个css文件解析为样式表对象，每个对象包含css规则，css规则对象包含选择器和声明对象，以及其他一些符合css语法的对象。  

解析器会将CSS文件解析成StyleSheet对象，且每个对象都包含CSS规则。CSS规则对象包含选择器和声明对象，以及其他与CSS语法对应的对象。

解析器先创建一个CSSStyleSheet对象，然后在对样式进行解析时，创建CSSStyleRule，并添加到已解析的样式对象列表中。解析完后，样式表中的所有样式规则都转化成了Webkit的内部模型对象CSSStyleRule对象。

所有的CSSStyleRule对象都存储在CSSStyleSheet对象中。然后将CSSStyleSheet对象转换为CSSRuleSet对象。后面就基于这些CSSRuleSet对象来决定每个页面中的元素样式。


### Webkit布局

## 渲染基础

### RenderObject树

### 网页层次和RenderLayer树

### 渲染方式

### Webkit软件渲染技术

## 硬件加速机制

## JavaScript引擎

### 概述
JavaScript引擎就是能够将JavaScript代码处理并执行的运行环境。
### V8引擎

### JavaScriptCore引擎
JavaScriptCore有六个构建块，用于分析，解释，优化和垃圾收集JavaScript代码。
### requestAnimationFrame接口（JavaScript引擎和渲染引擎Webkit的协同工作）

## 插件和JavaScript扩展

## 多媒体

## 安全机制

## 移动Webkit
