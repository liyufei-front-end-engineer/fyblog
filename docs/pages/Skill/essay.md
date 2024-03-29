# 随笔

## 白屏时间

白屏时间指的是指浏览器从响应用户输入网址地址，到浏览器开始显示内容的时间

- 白屏时间 = firstPaint - performance.timing.navigationStart
- 白屏时间 = firstPaint - pageStartTime



## 首屏时间

首屏时间是指用户打开网站开始，到浏览器首屏内容渲染完成的时间。对于用户体验来说，首屏时间是用户对一个网站的重要体验因素



计算首屏的方法有：

- 首屏模块标签标记法   firstScreen - performance.timing.navigationStart
- 统计首屏内加载最慢的图片的时间   加载最慢的图片的时间点 - performance.timing.navigationStart
- 自定义首屏内容计算法

## 白屏时间优化

1. 针对DNS部分的相关优化：
   + DNS 缓存优化，如减少 DNS 查询，避免重定向
   + DNS预加载策略，如设置 prefetch 对资源进行 DNS 预解析
   + 稳定可靠的DNS服务器
   + 域名发散：PC端浏览器有域名并发限制（chrome 为6个），可以将 http 静态资源放入多个域名、子域名中，以保证资源更快加载。常见的办法是使用 CDN
   + 域名收敛：将静态资源放在同一个域名下，减少 DNS 解析的开销。域名收敛是移动互联网的产物
   + HttpDNS：DNS 请求使用的是 UDP，容易被劫持、丢包，将 DNS 这种容易被劫持的协议，转而使用 HTTP 协议请求 Domain 与 IP 地址之间映射。获得正确的 IP 地址后，就不用担心 ISP 篡改数据，导致 DNS 解析失败等问题
2. TCP网络链路优化：
   + 增加带宽，提高网速
   + TCP 性能优化（偏计服务器设置：增大 TCP 的初始拥塞窗口、慢启动重启、窗口缩放、TFO 设置等等）
3. 服务端优化：
   + 数据层面：Redis 缓存、数据库缓存、服务端代码优化
   + 静态资源层面：资源压缩（Gzip 压缩等）
   + 服务器配置：
     - 协商缓存、强缓存
     - HTTP 1.1、HTTP 2.0
     - CDN
4. 前端优化



### 前端优化

1. 资源加载：

   + 按需加载，如 Vue 路由动态加载
   + CDN 加速
   + 图片懒加载

2. 减少资源大小：

   + 尽可能精简 HTML 代码和结构
   + 压缩 html、js、css 代码
   + 压缩图片，合并图片，小图片用 base64

3. 合理放置CSS文件和JS文件的加载位置，因为 JS 执行会阻塞 HTML 和 CSS 的渲染

4. 使用骨架屏，减少用户焦虑时长：静态图片、动态 DOM 等

5. 本地缓存： 

   - 通过 cookie、localStorage 以及一定的策略，对静态文件进行缓存，减少请求
   - 通过 HTML5 提供的新 API ，如 localhost/manifest 和 service worker 相关
   
6. SPA -> SSR：

   + 单页面应用往往会动态绘制 DOM ，即时静态资源都已加载，还需要创建 DOM，所以可以对一些时效性比较强的页面考虑使用 SSR，服务端渲染
   + 服务端渲染（SSR）：压力会来到服务端，需要处理好服务端的数据吞吐性能，做好扩容和相关缓存的优化
   + 还有一种基于两者之间的方案，页面预渲染，prerender-spa-plugin 方案
7. 前端代码自身的优化，减少耗时长的代码逻辑，提高代码质量

   

   



## 性能相关的知识

前端性能相关的指标，除了页面白屏时间之外，影响整个网站站点的性能及体验还有其他多个方面的指标，参考如下： 

- FP（First Paint）：首次绘制时间，包括了任何用户自定义的背景绘制，它是首先将像素
- FCP（First Content Paint）：首次内容绘制。浏览器将第一个 DOM 渲染到屏幕的时间，可能是文本、图像、SVG 等，这其实就是白屏时间。
- FMP（First Meaningful Paint）：首次有意义绘制。页面有意义的内容渲染时间。
- LCP（Largest Contentful Paint）：最大内容渲染。代表在 viewport 中最大的页面元素加载的时间。
- DCL（DomContentLoaded）：DOM  加赞完成。当 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发。无需等待样式表、图像和子框架的完成加载。
- TTI（Time to Interactive）：可交互时间。用于标记应用已进行视觉渲染并能可靠响应用户输入的时间点。
- FID（First Input Delay）：首次输入延迟。用户首次和页面交互（单击链接、点击按钮等）到页面响应交互的时间。

前端监控，除了被动的发现问题，然后解决问题，还可以建立一套数据监控平台，用数据来发现问题，来证明优化的效果（本文仅提供方向）： 

- 异常监控： 
  - 如果首屏报错，也会导致页面无法正常显示
  - 核心手段：window.onerror，vue 框架的 errorHandler，react 框架的 componentDidCatch 等
- 性能监控：给出合理的监控数据，反向定位白屏时间出现的原因 
  - 常见的直播如上述的性能指标
  - 制定团队关系的自定义指标
  - 基于 performance api 的精确化打点
