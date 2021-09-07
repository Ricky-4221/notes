## 基于 TAM & RUM 的前端优化理论与实践 - 性能篇（一）

导语 前端监控 TAM 是一站式前端监控解决方案，目前在公司内部前端团队广泛应用，取得了不少同事的认可，基于 TAM 打造的腾讯云前端性能监控 Real User Monitoring (https://cloud.tencent.com/product/rum) 目前也已经上线，欢迎大家体验和反馈。

很多同学接入 [TAM](https://tam.woa.com/) 和 [RUM](https://cloud.tencent.com/product/rum) 都是为了对自己的页面性能做一个质量评价，通过第三方的质量评价体系来验证自己项目的性能和质量如何，因此了解页面性能指标显得格外重要。通俗点说，某人说自己页面页面快，是否快，究竟有多快，怎么衡量，需要一个中立的裁判来裁决，在这里 TAM & RUM 的“页面性能”就是这个裁判。

本文会结合前端监控SDK源码 - Aegis 和 Google 最新的页面性能规范来跟大家一一讲解前端性能指标的规范，以及如何看懂 TAM & RUM 可视化图表，并且针对性的对您的项目进行优化。欢迎有兴趣的同学一起讨论留言。



### 页面性能指标有哪些

白屏时间，首屏时间、FCP、FMP、LCP、FID、TTFB... 这些名词太多，水太深，一般人把握不住。我们从中抽取一些最常见，最实用的规范跟大家一一解释。

#### 网络连接瀑布图(TL;DR)

要解释这些指标，还是要先祭出网络连接接瀑布图，想必只要对页面性能稍有了解的同学都见过这张图。

![img](https://km.woa.com/files/photos/pictures/202109/1630835918_34_w1473_h879.png)

![img](https://wq.360buyimg.com/data/ppms/picture/timestamp_diagram_2019_11_08_11_10.svg)

与这张图一一对应的，是浏览器里面的 `performance.timing` 属性，我们同时打印出来，做一个数据的对比说明。

![img](https://km.woa.com/files/photos/pictures/1aa/8a3ad651f02a464cd5eb209bc70d7_w1446_h812.png)

- navigationStart: 表示从上一个文档卸载结束时的 unix 时间戳，如果没有上一个文档，这个值将和 fetchStart 相等。
- unloadEventStart: 表示前一个网页（与当前页面同域）unload 的时间戳，如果无前一个网页 unload 或者前一个网页与当前页面不同域，则值为 0。
- unloadEventEnd: 返回前一个页面 unload 时间绑定的回调函数执行完毕的时间戳。
- redirectStart: 第一个 HTTP 重定向发生时的时间。有跳转且是同域名内的重定向才算，否则值为 0。
- redirectEnd: 最后一个 HTTP 重定向完成时的时间。有跳转且是同域名内部的重定向才算，否则值为 0。
- fetchStart: 浏览器准备好使用 HTTP 请求抓取文档的时间，这发生在检查本地缓存之前。
- domainLookupStart/domainLookupEnd: DNS 域名查询开始/结束的时间，如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等
- connectStart: HTTP（TCP）开始/重新 建立连接的时间，如果是持久连接，则与 fetchStart 值相等。
- connectEnd: HTTP（TCP） 完成建立连接的时间（完成握手），如果是持久连接，则与 fetchStart 值相等。
- secureConnectionStart: HTTPS 连接开始的时间，如果不是安全连接，则值为 0。
- requestStart: HTTP 请求读取真实文档开始的时间（完成建立连接），包括从本地读取缓存。
- responseStart: HTTP 开始接收响应的时间（获取到第一个字节），包括从本地读取缓存。
- responseEnd: HTTP 响应全部接收完成的时间（获取到最后一个字节），包括从本地读取缓存。
- domLoading: 开始解析渲染 DOM 树的时间，此时 Document.readyState 变为 loading，并将抛出 readystatechange 相关事件。
- domInteractive: 完成解析 DOM 树的时间，Document.readyState 变为 interactive，并将抛出 readystatechange 相关事件，注意只是 DOM 树解析完成，这时候并没有开始加载网页内的资源。
- domContentLoadedEventStart: DOM 解析完成后，网页内资源加载开始的时间，在 DOMContentLoaded 事件抛出前发生。
- domContentLoadedEventEnd: DOM 解析完成后，网页内资源加载完成的时间（如 JS 脚本加载执行完毕）。
- domComplete: DOM 树解析完成，且资源也准备就绪的时间，Document.readyState 变为 complete，并将抛出 readystatechange 相关事件。
- loadEventStart: load 事件发送给文档，也即 load 回调函数开始执行的时间。
- loadEventEnd: load 事件的回调函数执行完毕的时间。

根据上述的定义，我们总结出来常见的页面指标的计算公式

```javascript
// 计算加载时间
getPerformanceTiming() {
  const t = performance.timing;
  const times = {};
  // 页面加载完成的时间，用户等待页面可用的时间
  times.loadPage = t.loadEventEnd - t.navigationStart;
  // 解析 DOM 树结构的时间
  times.domReady = t.domComplete - t.responseEnd;
  // 重定向的时间
  times.redirect = t.redirectEnd - t.redirectStart;
  // DNS 查询时间
  times.lookupDomain = t.domainLookupEnd - t.domainLookupStart;
  // 读取页面第一个字节的时间
  times.ttfb = t.responseStart - t.navigationStart;
  // 资源请求加载完成的时间
  times.request = t.responseEnd - t.requestStart;
  // 执行 onload 回调函数的时间
  times.loadEvent = t.loadEventEnd - t.loadEventStart;
  // DNS 缓存时间
  times.appcache = t.domainLookupStart - t.fetchStart;
  // 卸载页面的时间
  times.unloadEvent = t.unloadEventEnd - t.unloadEventStart;
  // TCP 建立连接完成握手的时间
  times.connect = t.connectEnd - t.connectStart;
  return times;
}
```



#### RUM & TAM 性能指标

了解了上述的基本知识，下面我们来看下 RUM & TAM 里面都用到了哪些指标。

首先是页面加载瀑布图，瀑布图是表示网站资源如何下载、由引擎解析的图表，它让我们可以查看资源之间的顺序和依赖关系。 有助于确定加载过程中发生重要事件的位置，还可以让用户轻松看到他们的网站性能的好坏，从而准确显示哪些速度正在减慢网站性能。（摘自 [dotcom-monitor](https://www.dotcom-monitor.com/blog/zh-hans/2020/05/25/优化网络-性能-理解-瀑布-图表/) ）

![img](https://km.woa.com/files/photos/pictures/4cb/10059180378badc33839e1bc5c892_w2744_h986.png)

Aegis SDK 源码中对其的计算规则如下

```javascript
const t: PerformanceTiming = performance.timing;
if (!t) return;
// 这里不知道为什么有时候 t.loadEventStart - t.domInteractive 返回一个很大的负数，暂时先简单处理
let resourceDownload = t.loadEventStart - t.domInteractive;
if (resourceDownload < 0) resourceDownload = 1070;
result = {
  dnsLookup: t.domainLookupEnd - t.domainLookupStart,
  tcp: t.connectEnd - t.connectStart,
  ssl: t.secureConnectionStart === 0 ? 0 : t.requestStart - t.secureConnectionStart,
  ttfb: t.responseStart - t.requestStart,
  contentDownload: t.responseEnd - t.responseStart,
  domParse: t.domInteractive - t.domLoading,
  resourceDownload
};
```

备注： `resourceDownload` 有时会出现一个极大的负数，所以做了简单兼容，取了几个项目的平均值。其他的页面性能指标计算规则大家可以通过代码比较直观的看出来。



RUM & TAM 里面有一个首屏时间，Aegis SDK 是如何计算的呢？

\1. 默认通过 MutationObserver 这个 API 来监控浏览器 document 对象的 DOM 变化，只计算在首屏内的 DOM 元素，把 DOM 变化时间作为 x 轴，单位时间内 DOM 变化的数量作为 y 轴，绘制曲线后，我们找到 DOM 变化最高点，认为是首屏完成。

\2. 如果开发者觉得该算法不准确，希望自己标记 DOM 元素，可以添加属性 <div `AEGIS-FIRST-SCREEN-TIMING`></div>，把某个元素识别为首屏关键元素，SDK 认为只要用户首屏出现此元素就是首屏完成。也可以添加属性 <div `AEGIS-IGNORE-FIRST-SCREEN-TIMING`></div>，把该 DOM 列入黑名单。

![img](https://km.woa.com/files/photos/pictures/377/0875fe2215c5bae98f5fcce13964b_w2740_h296.png)

除了上述的数据外，RUM & TAM 还根据上报的数据计算了以上几个页面性能相关的指标。

\1. 首字节(TTFB) = DNS + SSL +TCP + TTFB

\2. DOM Ready = DNS + SSL +TCP + TTFB + ContentDownload + DomParse

\3. 页面完全加载 =DNS + SSL +TCP + TTFB + ContentDownload + DomParse + ResourceDownload



#### web-vitals

上述计算首屏的算法是 Aegis SDK 自主提供的算法，用户场景千变万化，确实有些场景没办法覆盖到，而且这个算法也无法得到所有开发者的认同。这个时候就需要祭出 web-vitals 了。

什么是Web Vitals？Google 给的定义是这是一个良好网站的基本指标 (Essential metrics for a healthy site)，为什么还要再定义一个新的指标集，原因是过去要衡量一个好的网站，需要使用的指标太多，推出 Web Vitals 是简化这个学习的曲线，站主只要关注 Web Vitals 指标表现即可（摘自 [知乎](https://zhuanlan.zhihu.com/p/149662237)）。

![img](https://km.woa.com/files/photos/pictures/1a5/3eb8bc6d1da49b4dbc60761aae535_w2746_h734.png)

目前 Google 的 [web-vitals 源码](https://github.com/GoogleChrome/web-vitals) 中提供了5个指标，分别为

\1. [CLS](https://web.dev/cls/)（Cumulative Layout Shift - 累积布局移位）: CLS 会衡量在网页的整个生命周期内发生的所有意外布局偏移的得分总和。得分是零到任意正数，其中 0 表示无偏移，且数字越大，网页的布局偏移越大。

\2. [FCP](https://web.dev/fcp/)（First Contentful Paint - 首次内容绘制）：FCP 度量从页面开始加载到页面内容的任何部分呈现在屏幕上的时间，页面内容包括文本、图像（包括背景图像）、<svg>元素或非白色的<canvas>元素。

\3. [FID](https://web.dev/fid/)（First Input Delay - 首次输入延迟）：从用户首次与您的网页互动（点击链接、点按按钮，等等）到浏览器响应此次互动之间的用时。这种衡量方案的对象是被用户首次点击的任何互动式元素。

\4. [LCP](https://web.dev/lcp/)（Largest Contentful Paint - 最大内容绘制）：LCP 度量从用户请求网址到在视口中渲染最大可见内容元素所需的时间。最大的元素通常是图片或视频，也可能是大型块级文本元素。

\5. [TTFB](https://web.dev/time-to-first-byte/) (Time To First Byte - 从服务器接收到第一个字节耗时) TTFB 是发出页面请求到接收到应答数据第一个字节的时间总和，它包含了 DNS 解析时间、 TCP 连接时间、发送 HTTP 请求时间和获得响应消息第一个字节的时间。

目前 RUM & TAM 采有了其中最重要的三个属性：LCP，FID，CLS。



这里可以看出与之前 “首屏时间” 比较模糊的定义不同，Google 对 web-vitals 给出了非常明确的指标定义，并且官方提供了算法支持，那么我们是不是可以直接用 LCP 取代我们自己写的 “首屏算法” 呢？目前显然还是不可行的，由于 LCP 底层使用的是 PerformanceObserver ，还存在兼容性问题，因此短期内还无法完全替代。

![img](https://km.woa.com/files/photos/pictures/ab5/d3e08dcb3c3b034f5fbe401548023_w2862_h1146.png)



不过，在可预见的未来，web-vitals 会成为业界的主流衡量标准，到那个时候，我们也可以卸下历史包袱，全面拥抱开源算法了。



### 通过数据指导开发优化

有了 RUM & TAM 这个好用的工具，下面就可以用数据指导开发和决策了。

如果某位开发者将自己的项目接入到 RUM & TAM 后，怎么样根据这些已有的数据来优化这些项目呢？

拿之前某团队邀请我们对其项目做的一次针对性优化举例。

![img](https://km.woa.com/files/photos/pictures/b03/543e1c988dd2df751a08451024b81_w2778_h1750.png)



首先开发者的核心诉求是页面响应快，性能好。而上图这个数据无论如何都称不上快，可以看到首屏时间达到了 4.8s，LCP 的时间超过了 4s，仅仅达到了 “POOR” 的级别，CLS 的数据也不容乐观。

我们首先仅从表面数据进行分析，对比了该项目下全部页面的数据，通过 “Top访问页面” tab 对页面进行分析，先按照 “首屏时间” 倒排序。

![img](https://km.woa.com/files/photos/pictures/6e2/99cc0c1a2225dafdf583a0ae58366_w2750_h610.png)

这里发现了第一个问题，开发者把多个项目的页面都使用了同一个上报ID进行上报，导致一些比较差的页面对整体数据产生了影响，因此我们建议用户尽可能根据代码组织和业务组织的方式区分不同的上报ID，方便定点发现问题。

排除了页面的干扰，我们再分析一下网络和区域干扰。

![img](https://km.woa.com/files/photos/pictures/377/ae60b9d237e1065d71c5f3827cfbb_w2754_h1300.png)

![img](https://km.woa.com/files/photos/pictures/7c3/8a7ad965c91aa2ca72ce42fce2220_w2750_h1300.png)

从上图可以看出来，网络状况和地区差异对页面首屏数据影响都不是很大。

再回到前面的瀑布图，从图中可以看到该项目主要的瓶颈其实在 “资源加载” 的耗时。通过对用户页面的资源加载情况进行分析，立刻找到了原因。

![img](https://km.woa.com/files/photos/pictures/381/ed19d4fbeb11f88dbf846b5a27229_w1652_h618.png)

用户使用的是 React 框架，在没有服务端渲染的情况下，页面是会在加载主 JS 后才渲染的，而用户大部分 JS 文件都打包成一个 bundle ，导致产生了一个超大的 JS 文件，这个 JS 文件就成为了用户页面渲染的瓶颈。除此之外还发现了该 JS 文件没有支持 HTTP2 协议。



#### 资源加载优化

于是联系用户做下面几件事

\1. 拆包，通过把公共外部依赖打包成为 vendor，并且对组件做异步加载。

\2. 去掉一些非必需的包，比如用户引入了全量的 lodash，让其改成 lodash-es，方便 webpack 做 treeshaking；去掉仅为了把某个时间做格式化而引入的 moment；去掉 jquery，而当初引入 jquery 仅仅为了查询某个元素而，真是得不偿失。

\3. 建议使用 webpack-bundle-analyzer 对打包后的代码进行分析，查看哪些包不需要引用，或者可以单独打包。

\4. 网络协议方面全面引入 HTTP2，合并了一些小的静态资源，把一些小的 svg 改成了 base64。



通过简单的分析，找到页面性能洼地，用户根据建议简单优化，效果十分显著，全量发布后不久数据就得到大幅提升，首屏时间从 4.8s 优化到 3.2s，最重要的是 “资源加载” 耗时直接减半。

![img](https://km.woa.com/files/photos/pictures/6e8/7d523c08a6ccfb3db98bb4a7e0789_w2752_h996.png)



然后发现了另外一个问题，用户的 “资源加载” 时间已经大幅度降低了，但是为什么 “首屏耗时” 没有相应的**同比降低**呢？

我们通过对用户页面分析发现，该页面在加载完成后，会执行非常多的 JS 代码逻辑，包括一些数据上报，用户行为收集，还有加载侧边栏，弹出广告等。这里带来了 2 个问题。

\1. 页面**主进程阻塞严重，**Aegis SDK 的一些逻辑在执行的时候受到了影响，导致实际执行时间要晚于设定的时间，所以上报的“首屏耗时”其实要比实际晚的。

\2. 用户的页面会在首屏完成后，**继续加载很多DOM元素**，也就是有很多 DOM 元素的变化，导致了 Aegis SDK 计算出来的首屏时间也要晚于真实的“首屏时间”。

于是我们建议用户把一些非必要操作都放在定时器中执行，以提升页面性能，提升用户体验。用户根据我们的建议通过定时器和异步改造，又大幅度提升了页面的“首屏时间”。

![img](https://km.woa.com/files/photos/pictures/707/49575343eb70b56b32278f758edf6_w2750_h988.png)

这个时候的“首屏耗时”已经是优化之初的 1/2 了，对于大多数用户来说，50% 的性能提升其实已经可以去交差了，但是我们看到用户另外几个指标依然不是很完美。其中 CLS 的得分一直是 “POOR” 的状态。



#### CLS 指标优化

CLS 指的是页面布局偏移量，再次简单分析，我们发现用户有一个长列表是页面主要渲染内容，该列表存在的问题是：因为数据不多，一般在 4 - 10 条数据，所以开发者没有对列表做分页。

没有分页带来的问题是，列表无法在渲染之初就确定长度，导致获取数据后渲染列表的时候页面发生较大的偏移，同时也带来了超多的 DOM 变化。

这个是导致 CLS 大的核心原因，当然也带来了 “首屏耗时” 的同步增加，除此之外，前面提到的一些异步数据，如广告挂件等也带来了这个问题。

给用户的建议如下：

\1. 在一开始就确定列表高度（加入分页），通过骨架屏优化加载效果，同时减少 DOM 变化。

\2. 广告挂件使用绝对布局，使其脱离文档流，减少DOM变化。

\3. 一些其他元素，如图片等，确定长度和宽度属性，这些值允许浏览器在将图像渲染到位之前保留视觉空间。

\4. 一些元素的变化，通过 CSS 实现，而不是使用 JS 改变元素属性实现。



再次优化后用户页面首屏和 CLS 数据变化惊人，达到了业界主流水平。最后我们看一下整体数据效果。

![img](https://km.woa.com/files/photos/pictures/a3c/6a6217368f9297f1cd0a117e2619c_w2748_h998.png)

![img](https://km.woa.com/files/photos/pictures/dc1/30d29eaf0cc3791803676e62f411e_w3102_h736.png)

就目前的数据来看，用户页面性能仍然有可提升空间，更深层次的优化需要借助 Chrome Performance 工具进行了，这个我们会另开一篇文章进行讲解和分析。



### 总结

以上仅仅是我们使用 RUM & TAM 做的优化的一个非常小的点，其中涉及到的知识都是前端开发耳熟能详的，通过好用的工具，指导开发同学做决策，从而达到优化的效果。引用 Lord Kelvin 的一句名言，**如果你无法衡量它，你就无法提升它。**

![img](https://km.woa.com/files/photos/pictures/b0d/6ab55ec7148c45865e32b47ab5615_w850_h400.jpg)

最后，希望大家都可以通过使用 RUM 和 TAM 来提升项目性能，早日晋升。