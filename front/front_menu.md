[TOC]

# 前端

## 基础

#### 1.概述

HTML:在浏览器中生成DOM节点树

CSS:给HTML元素添加样式，匹配方式：DOM元素（如p） class id

Javascript处理事件（点击、输入）改变HTML的内容 处理HTTP请求 执行各种业务逻辑，因为对页面交互的同步处理所以是单线程，作为浏览器脚本语言，主要用途是与用户互动，以及操作DOM，多线程会导致严重的同步问题

#### 2.SVG

##### 概念

可缩放矢量图形Scalable Vector Graphics

使用XML格式来定义图形

主要优点有：1.改变尺寸时图形质量不会损失 2.可以在任何分辨率下被高质量打印 3.相比于JPEG和GIF，尺寸更小，可压缩性更强 4.文本可选、可搜索（适合制作地图） 5.与其他标准（如XSL DOM）兼容

![image-20210825160850554](C:\Users\rickyrqzhao\AppData\Roaming\Typora\typora-user-images\image-20210825160850554.png)

可以通过<embed> <object> <iframe>标签来把SVG文件嵌入HTML文档，写明src="circle.svg" 前两个还要求写type="image/svg+xml" object标签不允许使用脚本，iframe只在H5中使用

还可以通过<svg>直接嵌入代码		

##### 常见图形和使用	

1.矩形 x属性是图形到浏览器窗口左侧的距离 y为离浏览器顶端，width height宽高，rx ry圆角 style:"fill:red"

2.圆形<circle> cx cy圆心坐标 r半径

3.椭圆<ellipse> cx cy圆心坐标 rx ry水平和垂直半径

4.直线<line> x1 y1 x2 y2 起点和终点坐标

5.多边形<polygon> points="200,10 250,190 160,210"定义每个角的坐标

6.多段线<polyline>

7.路径<path>

8.文本<text>

##### 其他属性

1.图形填充规则：fill-rule属性，指定如何判断图形的内部

nonzero 从该点做射线，路径从左往右经过的个数和从右往左经过的个数，相等为外部

evenodd 从该点做射线，与图形路径的交点个数，偶数为外部

2.定义线条和轮廓的stroke属性

stroke="red" 颜色 stroke-width厚度 stroke-linecap线条端点形状(round/square/butt) stroke-dasharray虚线

3.此外还支持滤镜、模糊效果、阴影、渐变

#### 3.前端框架是什么 

前端框架是封装一些功能代码以简化网页设计的工具，可以减少开发的工作量，同时提升兼容性和美观度

vue就是一套构建用户界面的渐进式框架

jquery针对页面上的DOM请求、远程请求、数据处理等都做了封装，但它不会影响代码结构，所以不是框架而是一个javascript库

框架对代码结构有规定，以模型为中心，DOM操作只是附加，通过关注点分离鼓励改进应用程序，随着前端功能的增强（往应用方向发展，web page与web app）而产生

未来的发展趋势是前后端只靠json数据进行通信，后端只处理和发送一段json到前端，计算和模板渲染都在前端进行，后台程序不再做模板的任何处理。使用MVC（详见基础-16）框架能有效实现前后端的解耦，简化开发流程，便于维护管理，可以把精力更多放到业务逻辑，提升开发效率。

#### 4.XSS

XSS（跨站脚本攻击）是指攻击者在返回的HTML中嵌入javascript脚本

XSS整个攻击过程大概为：利用页面中的用户输入内容，拼接特殊格式的字符串，突破原有的位置限制（指用户输入内容都在固定容器和属性内），形成代码片段，从而在网站上注入脚本

避免的方法主要是将用户所提供的内容进行过滤，如vue中数据绑定（双大括号，v-bind）会进行HTML转义，默认将数据解释成普通文本而非代码，当然可以用v-html指令输出

cookie的防范为是需要在HTTP头部配上，set-cookie：

httponly-这个属性可以防止XSS,它会禁止javascript脚本来访问cookie。

secure - 这个属性告诉浏览器仅在请求为https的时候发送cookie。

结果应该是这样的：Set-Cookie=<cookie-value>.....

#### 5.数据绑定

指使用双大括号来绑定变量的方式

文本插值<div>{{ data }}</div>

new Vue({

  data:{

​    data:'test'

  }

})

实现过程：

1.解析语法生成AST（Abstract Synax Tree 抽象语法树)

thisDiv = {

  dom:{

​    type: 'dom',  ele: 'div',  nodeIndex: 0,  children:  [{ type: 'text',  value: '' }]

  }

  binding: [{  type: 'dom',  nodeIndex: 0,  valueName: 'data'}]

}

2.根据AST结果生成DOM

3.将数据绑定更新到模板

#### 6.虚拟DOM

1.用JS对象模拟DOM树，得到一棵虚拟的DOM树

2.当页面数据发生变更时，生成新的虚拟DOM树，比较新旧两棵树的差异

3.把差异应用到真正的DOM上

使用原因：一个DOM节点包含很多很多属性、元素、事件，但我们并不是全都会用到，所以在通常的节点内容、位置、样式和增删的操作中，用JS对象来表示DOM元素，可以降低比较两个节点差异的计算量

![image-20210708140757389](file://C:\Users\rickyrqzhao\AppData\Roaming\Typora\typora-user-images\image-20210708140757389.png?lastModify=1629430692)

#### 7.事件驱动的编码方式

1.编写静态页面

2.给元素绑定对应的事件 

document.getElementById('a').addEventListener('input',function1);

3.事件触发时更新页面的内容

#### 8.数据驱动的编码方式和优点

1.设计数据结构

export default{

  data(){

​    return {  name: ' ',  email: ' '  }

  }

}

2.将数据绑定到页面中需要使用、展示的地方

<p>{{  name  }}</p>

3.事件触发时，更新数据

methods: {

  updateNameValue(event){

​    this.name=event.target.value;

  }

}

在使用数据驱动的时候，模板渲染的事情交给框架完成，程序员只需要进行数据处理。

数据驱动的好处是，当页面抽象成数据来表示时，可以做的事情就很多了，如果把所有页面、组件、展示内容和接口配置都设置成配置数据，就可以把所有东西都进行配置化

组件的配置——通过配置生成表单

页面应用的配置——通过配置生成管理端

vue和angular都可以做到，配置化的难度不在于技术而在于如何把业务进行适当的抽象

#### 9.关于前端工程化

主流构建工具：webpack

nodejs的包管理器 npm

yarn同理 区别是安装时速度更快（并行、离线等），版本统一管理得比较好

脚手架 vue cli 快速生成示例代码、安装依赖

#### 10.比较前沿的前端技术（更新于2020年末）

a.2020年Deno超过vue.js成为过去一年star增长最快的开源项目，打破了vue.js连续4年登顶的垄断

b.TypeScript爆发性增长，在2020年github语言排行榜第四

c.根据项目分布的情况，三大框架论生态圈React>Vue>Angular

d.2020年发布了vue.js3.0 完全ts重构，是源码易于阅读和维护，重写了虚拟dom的实现，优化编译模板，提升组件初始化的性能，composition api基于函数理念解决vue2中对象式组件存在的一些问题，使用函数将统一逻辑的组件代码收拢在一起达到复用，也有利于构建时的tree-shaking检测

e.WASM WebAssembly 设计补充JavaScript 在19年成为WorldWide Web Consortium W3C的标准，成为浏览器官方标准的第四门语言，其本解码速度比js解析块，让高性能的web应用在浏览器上允许成为可能，可在浏览器中的专有虚拟机上执行，与js虚拟机共享内存和线程等资源

f.Low-Code 低代码

no-code：编程给自己用，给用户的感觉是一个软件，使用图形化操作，如ppt excel

low-code：编程给其他人用，通过降低专业难度，让产品和运营人员参与，平台预估场景和需求，降低从头写代码的成本

pro-code：日常的手写代码

g.DevOps 开发运维一体化流程 CI/CD流程的串联 帮助业务提升研发效能

h. WebRTC

新一代国际视频编解码标准H.266/VVC出炉

#### 11.CSRF攻击 b站是如何获取a站的cookie的

#### 12.前端安全

#### 13.异步编程都有哪些

#### 14.BOM DOM

##### BOM浏览器对象

常见的Bom属性有

1.location对象

location.href-- 返回或设置当前文档的URL
location.search -- 返回URL中的查询字符串部分。例如 http://www.dreamdu.com/dreamdu.php?id=5&name=dreamdu 返回包括(?)后面的内容?id=5&name=dreamdu
location.hash -- 返回URL#后面的内容，如果没有#，返回空
location.host -- 返回URL中的域名部分，例如[www.dreamdu.com](http://www.dreamdu.com/)
location.hostname -- 返回URL中的主域名部分，例如dreamdu.com
location.pathname -- 返回URL的域名后的部分。例如 http://www.dreamdu.com/xhtml/ 返回/xhtml/
location.port -- 返回URL中的端口部分。例如 http://www.dreamdu.com:8080/xhtml/ 返回8080
location.protocol -- 返回URL中的协议部分。例如 http://www.dreamdu.com:8080/xhtml/ 返回(//)前面的内容http:
location.assign -- 设置当前文档的URL
location.replace() -- 设置当前文档的URL，并且在history对象的地址列表中移除这个URL location.replace(url);
location.reload() -- 重载当前页面

2.history对象

history.go() -- 前进或后退指定的页面数 history.go(num);
history.back() -- 后退一页
history.forward() -- 前进一页

3.navigator对象

navigator.userAgent -- 返回用户代理头的字符串表示(就是包括浏览器版本信息等的字符串)
navigator.cookieEnabled -- 返回浏览器是否支持(启用)cookie

#### 15.REST和RESTful架构

REST Representational State Transfer 表征性状态转移 是一组架构约束条件和原则

如果一个架构符合REST，就称他为RESTful架构

一句话概括：用URL定位资源，用HTTP描述操作

#### 16.MVC MVVM

MVC是前后端的交互方式，MVVM是页面渲染的方式

![img](https://upload-images.jianshu.io/upload_images/1512918-8aa332c6d3ec0a32.png?imageMogr2/auto-orient/strip|imageView2/2/w/602/format/webp)

MVC model view controller 模型，视图，控制器

![img](https://upload-images.jianshu.io/upload_images/1512918-1b967be216171088.png?imageMogr2/auto-orient/strip|imageView2/2/w/407/format/webp)

![img](https://upload-images.jianshu.io/upload_images/1512918-f01d27a13fec37ea.png?imageMogr2/auto-orient/strip|imageView2/2/w/503/format/webp)

view model视图模板，存储视图所处的状态 ，view和view model的属性数据双向绑定

controller负责model的初始化和操作

## 浏览器

#### 1.浏览器渲染页面时解析文件的顺序

1.解析HTML/SVG/XHTML，生成DOM结构树

2.解析CSS，生成CSS规则树

3.解析JS，通过DOM API和CSS API来操作1 2中的树

#### 2.浏览器绘制页面的过程

1.计算CSS规则树

2.生成Render树（CSS树和DOM树的结合，即最终呈现的页面，例如会移除DOM中匹配到CSS设置了display:none的节点）（display:none详见CSS-1 2）

3.根据CSS属性计算各个节点的大小，如position z-index

4.绘制

#### 3.页面局部刷新的两种方式（指该变内容）

1.绑定映射表 

var a = document.getElementById('a') // 获得元素映射

a.innerHTML = '<p>test<p>' //a有了一个子标签p

a.appendChild(document.createElement('b'));// 又加了一个b

a.removeChild(a.firstChild);//在这里是第一行加入的p被删掉

2.直接替换内容

a.innerHTML = '<p>test</p>';

a.innerHTML = '<p>test</p><b></b>'

a.innerHTML = '<b></b>'

#### 4.回流和重绘

Repaint 部分重画，只触发绘制的第四步，不改变尺寸只改变颜色

Reflow 节点需要重新计算和绘制（如改变尺寸） 触发第三第四步 也只重新构造受影响的部分渲染树

#### 5.解析“-”

在使用点运算符的时候，浏览器无法正确解析“-”，所以要么使用驼峰，要么加方括号，方括号里“-”被理解为字符串中的内容，所以可以正确解析

例如style.backgroundColor style["background-color"]都可以 style.backgroound-color不可以

#### 6.实现登录状态保持的两种方法 cookie token

a.cookie session的配合使用

用户第一次输入用户名和密码之后，前台发送post请求，后台获取用户信息，查询数据库验证，验证通过之后开辟一块session空间存储用户数据，同时生成一个cookie字符串，由后台返回给前台，存储在浏览器的cookie空间，下次客户端自动携带这个cookie去请求服务器，服务器识别后就能读取session中的用户信息，从而实现用户的直接访问 

优点是提升用户体验，且比客户端直接保存用户信息要相对安全

缺点是服务器向浏览器传cookie的时候很容易被劫持，大型项目中服务器不止一台，请求被分到不同的服务器就要重新登录

由于HTTP是一个无状态协议所以cookie最大的作用就是存储session id用来唯一标识用户

b.使用token安全令牌

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20190610150742663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1lfUmFjaGVs,size_16,color_FFFFFF,t_70)

当用户请求页面，输入信息，服务端验证之后生成token安全令牌，一个随机字符串，返回给客户端，客户端下一次请求直接携带这个token就能被服务端识别

优点是不需要服务器再开辟空间，相对更安全，即使传输过程中被劫持，别人也不能破解内容，并且减少了服务器压力，避免频繁查询数据库

#### 7.如何实现上传文件

由于浏览器不能直接操作文件系统，需要由用户主动授权访问，读取文件内容到指定的内存，最后执行提交请求，将内存里的数据上传到服务端

form method="POST" enctype="multipart/form-data"

<input type="file" name="file" value="请选择文件">

<input type="submit">

</form>

表单数据编码格式设置成multipart/form-data，会对文件内容在上传时进行处理以便服务端处理程序解析文件类型与内容

但由于form表单提交操作会造成整体刷新所以一般不用，一般使用异步操作请求AJAX，即浏览器的XMLHttpRequest自定义提交

因为没有设置编码，所以需要前端自行格式化文件内容

<input type="button" value="文件上传" onclick="uploadFile()">

script中function uploadFile(){

const file=document.getElementById('file').files[0];

const xhr=new XMLHttpRequest();

const fd=new FormData(); // 用浏览器自身提供的formdata构造函数来实例化一个文件fd

fd.append('file',file);//使用fd的append方法将文件内容插入进去

xhr.open('POST','http://127.0.0.1:8000/upload',true)

xhr.onreadystatechange=function(){...}

xhr.send(fd); //利用XMLHttpRequest实例发送

}

#### 8.从输入URL到页面显示的过程

a.DNS解析

b.TCP连接

c.发送HTTP请求

d.服务器处理请求并返回HTTP报文

e.浏览器接收服务器发来的HTML文档之后会遍历文档节点生成DOM树，解析CSS文件生成CSSOM规则树，渲染页面

#### 9.进程线程 浏览器的多线程和多进程

进程和线程的区别：

a.起源不同：60年代在操作系统中出现进程，80年代为了解决进程的弊端而出现了线程

b.概念不同：进程是并发执行的程序在执行过程中**系统**分配和管理资源的基本单位，是一个动态概念，可以理解为具有一定独立功能的程序，而线程是进程的一个实体，是**CPU**调度分派的基本单位

c.数量不同：一个程序至少有一个进程，一个进程至少有一个线程，系统把资源分配给进程，同一个进程下所有线程共享该进程的资源

d.资源共享方式不同：不同进程之间的内存不共享（如浏览器无法访问音乐播放器的内容），进程间访问内存需要进程间通信IPC，而线程本身都服务于同一个进程，两个线程之间可以直接访问共享内存，可以不通过内核进行通信

e.开销不同：线程的创建、终止时间比进程短，统一进程内线程切换时间比进程切换短

ps 线程共享的内容包括：进程代码段，进程的公有数据，进程打开的文件描述符，信号处理器，当前目录，而独有的内容包括：线程id，寄存器组的值，线程的堆栈，错误返回码，线程的信号屏蔽码



![img](https://images2015.cnblogs.com/blog/1014798/201609/1014798-20160907194136676-1930880313.png)

js是基于单线程的，这个线程就是浏览器的js引擎

在执行某些耗时操作（如加载图片）时js线程会被阻塞，不能允许后续代码，所以新开一个工作线程专门干这些耗时的加载 使用worker类 var worker = new Worker(参数是js文件的路径)  是js引擎向浏览器申请开子线程，子线程是浏览器开的，不能操作DOM 两个线程互不影响

使用onPostMessage(data)和onmessage回调函数

浏览器的渲染进程是多线程的，页面渲染，js执行，事件循环，都在这个进程里执行

界面GUI渲染线程包括解析HTML CSS 构建DOM树和RenderObject树 布局和绘制等

GUI线程与JS引擎线程是互斥的，如果JS执行时间过长，就会造成页面渲染不连贯

事件触发线程归属于浏览器，用来控制事件循环

（注意，setTimeout setInterval等浏览器定时计数器不是由js计数的，而是通过单独线程来计时并触发定时，此外规定要求低于4ms的时间间隔算为4ms）

异步HTTP请求线程：在XMLHttpRequest连接后通过浏览器单开一个线程请求，检测到状态变更时，如果设置有回调函数，异步线程就产生状态变更事件，将这个回调放入事件队列中，再由js引擎执行

浏览器打开一个网页就相当于新起一个进程，出于优化有时会将多个进程合并成一个（如打开多个空白页）

多进程的优势：1.避免单个page crash或第三方插件crash影响整个浏览器 2.充分利用多核的优势 3.方便使用沙河模型隔离插件等进程，提高浏览器的稳定性

#### 10.点击劫持里的iframe是如何内嵌到第三方网页上的

#### 11.如何实现授权功能

#### 12.域名解析的过程

#### 13.图片URL访问后直接下载如何实现

在请求的返回头里，用于浏览器解析的重要参数就算OSS的API文档中的返回HTTP头，决定了用户下载行为的参数

下载的情况下：

\1. x-oss-object-type:

Normal

\2. x-oss-request-id:

598D5ED34F29D01FE2925F41

\3. x-oss-storage-class:

Standard

#### 14.Web Quality无障碍

能被残障人士使用的网站才是易用、易访问的网站。

使用alt属性 <img src="person.jpg" alt="a person"

可供盲人和弱视人群使用的语音浏览器

#### 15.cookie的作用

保存用户登录状态。例如将用户id存储于一个cookie内，这样当用户下次访问该页面时就不需要重新登录了，现在很多论坛和社区都提供这样的功能。 cookie还可以设置过期时间，当超过时间期限后，cookie就会自动消失。因此，系统往往可以提示用户保持登录状态的时间：常见选项有一个月、三个 月、一年等。

跟踪用户行为。例如一个天气预报网站，能够根据用户选择的地区显示当地的天气情况。如果每次都需要选择所在地是烦琐的，当利用了cookie后就会显得很人性化了，系统能够记住上一次访问的地区，当下次再打开该页面时，它就会自动显示上次用户所在地区的天气情况。因为一切都是在后 台完成，所以这样的页面就像为某个用户所定制的一样，使用起来非常方便定制页面。如果网站提供了换肤或更换布局的功能，那么可以使用cookie来记录用户的选项，例如：背景色、分辨率等。当用户下次访问时，仍然可以保存上一次访问的界面风格。

#### 16.cookie  session sessionStorage localStorage的区别

共同点：都是保存在浏览器端，并且是同源的

Cookie：cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下,存储的大小很小只有4K左右。 （key：可以在浏览器和服务器端来回传递，存储容量小，只有大约4K左右）

session: HTTP是一个无状态协议，因此cookie最大的作用就算存储sessionId用来唯一标识用户

sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持，localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。（key：本身就是一个回话过程，关闭浏览器后消失，session为一个回话，当页面不同即使是同一页面打开两次，也被视为同一次回话）

localStorage：localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。（key：同源窗口都会共享，并且不会失效，不管窗口或者浏览器关闭与否都会始终生效）

#### 17.响应式布局/自适应解决方案

##### px和视口

一个像素表示计算机屏幕所能显示的最小区域，分为css像素（开发使用的一个抽象单位）和物理像素（只和设备的硬件密度有关，固定）

视口就是浏览器显示内容的屏幕区域，包括布局、视觉和理想视口

(1) 布局视口（layout viewport）

布局视口定义了pc网页在移动端的默认布局行为，因为通常pc的分辨率较大，布局视口默认为980px。也就是说在不设置网页的viewport的情况下，pc端的网页默认会以布局视口为基准，在移动端进行展示。因此我们可以明显看出来，默认为布局视口时，根植于pc端的网页在移动端展示很模糊。

(2) 视觉视口（visual viewport）

视觉视口表示浏览器内看到的网站的显示区域，用户可以通过缩放来查看网页的显示内容，从而改变视觉视口。视觉视口的定义，就像拿着一个放大镜分别从不同距离观察同一个物体，视觉视口仅仅类似于放大镜中显示的内容，因此视觉视口不会影响布局视口的宽度和高度。

(3) 理想视口（ideal viewport）

理想视口或者应该全称为“理想的布局视口”，在移动设备中就是指设备的分辨率。换句话说，理想视口或者说分辨率就是给定设备物理像素的情况下，最佳的“布局视口”。

在不缩放的情况下，一个css像素=DPR(Device pixel ratio) =物理像素/分辨率

移动端可以使用viewport元标签来控制布局

下面这个就可以实现移动端在理想视口下布局

initial初始缩放比例 min最小 max最大 user是否允许手动缩放

<meta id="viewport" name="viewport" content="width=device-width; initial-scale=1.0; maximum-scale=1; user-scalable=no;"

所以不同设备下一个css像素表示物理像素是不同的

##### 媒体查询

既然一套固定的样式不能实现各端的自适应，那么可以给每个设备一套不同的、对应的样式

方式为@media媒体查询

@media screen and (max-width: 960px){ //550~960

​	body{ background-color: #000000; }

}

@media screen and (max-width: 550px){ //0~550

​	body{ background-color: #000000; }

}

缺点是浏览器大小改变时需要改变的样式太多

##### 百分比



##### rem

##### vw/vh

## HTML

#### 1.新窗口打开网页 target="_blank" 加一个什么属性防止打到钓鱼网站

默认_self，在相同的框架中打开被链接的文档 _parent在父框架集中打开  _top在整个窗口中打开 

#### 2.图片的alt和title

alt和title同时设置时，alt作为图片的替代文字出现（在图片没加载完的时候看见），title是图片的解释文字

alt是img area等元素的属性，而title作为标签是网页的标题，作为属性时用来为元素提供额外的说明信息，例如给超链接标签a添加title属性，把鼠标移动到该链接上面，就会显示title的内容

#### 3.HTML5新定义的标签

audio定义声音，比如音乐和其他音频流

canvas定义图形，比如图表和其他图像，只是图像容器

menu定义命令列表，用于菜单、工具栏以及列出表单控件

command定义命令按钮，表示用户能够调用的命令，如单选，复选，按钮。只有当command元素位于menu元素内时该元素才是可见的，否则不会显示，但可以用它规定键盘快捷键    

article定义外部的内容，例如来自Blog、论坛等外部提供者的文本

#### 4.HTML5 drag api

dragstart：事件主体是被拖放元素，在开始拖放被拖放元素时触发，。

darg：事件主体是被拖放元素，在正在拖放被拖放元素时触发。

dragenter：事件主体是目标元素，在被拖放元素进入某元素时触发。

dragover：事件主体是目标元素，在被拖放在某元素内移动时触发。

dragleave：事件主体是目标元素，在被拖放元素移出目标元素是触发。

drop：事件主体是目标元素，在目标元素完全接受被拖放元素时触发。

dragend：事件主体是被拖放元素，在整个拖放操作结束时触发

#### 5.语义化标签

HTML5语义化标签是指正确的标签包含了正确的内容，结构良好，便于阅读，比如nav表示导航条，类似的还有article、header、footer等等标签。

#### 6.iframe

定义：iframe元素会创建包含另一个文档的内联框架

提示：可以将提示文字放在<iframe></iframe>之间，来提示某些不支持iframe的浏览器

缺点：

会阻塞主页面的onload事件

搜索引擎无法解读这种页面，不利于SEO

iframe和主页面共享连接池，而浏览器对相同区域有限制所以会影响性能。

#### 7.Doctype 严格和混杂模式

Doctype声明于文档最前面，告诉浏览器以何种方式来渲染页面，这里有两种模式，严格模式和混杂模式。

严格模式的排版和JS 运作模式是 以该浏览器支持的最高标准运行。

混杂模式，向后兼容，模拟老式浏览器，防止浏览器无法兼容页面。

## CSS

#### 1.深入display:none

设置了display:none的元素不会显示、不占布局空间，但仍然可以通过js操作。因为DOM树是浏览器解析HTML时就生成的，和CSSOM树合成Render树，元素在Render树中对应0或多个盒子，display:none的元素没有盒子，但DOM操作是可以的。

还有一些需要注意的点：

1.浏览器有一些原生默认display:none的元素，如link script style dialog 设置type=hidden的input

2.H5新增hidden布尔属性，让开发者自行定义元素的隐藏性，如

[hidden]{ display: none; }

<span hidden>等效于<span display:none>

3.父元素display:none 可以覆盖子孙元素的display设置，都看不见

4.如此设置的元素无法获得焦点，因为页面上根本没有

5.无法响应任何事件，在捕获、命中目标和冒泡阶段都不可以，因为无法获取焦点所以不是鼠标键盘的命中目标，且父子元素都是none所以也不会出现在捕获和冒泡的路径上

6.不影响form表单提交数据

<form> ...<input type="text" name="a" style="display:none">就算看不见元素但提交的时候还是会把该Input元素的值提交上去

7.counter（详见CSS-6）会忽略display:none的元素

8.display的变化不影响transition

9.display变化时会触发reflow回流

因为display就是更改元素采用的布局方式的

#### 2.visibility，display:none和visibility:hidden的对比

visibility的两个作用：隐藏表格的行和列，在不触发布局的情况下隐藏元素

四个有效值：

visible 显示 hidden 在页面上不可见但保留原占有的位置 inherit继承

collapse用于表格子元素（如tr tbody col colgroup）时效果和display:none一样，用于其他元素时和hidden一样，不过因为各浏览器实现结果有出入所以一般不用

对比display:none和visibility:hidden

1.父元素为hidden时子元素设置visible可以生效 而父元素如果为none则子元素一定none

2.都无法获得焦点

3.hidden可以在冒泡阶段响应事件，因为他的子元素可以visible

4.都不妨碍表单提交

5.counter不会忽略hidden 但忽略none

6.transition对visibility的变化有效 对display无效

7.visibility的变化不会触发reflow 因为不会改变元素布局相关的属性，只是和其他渲染变化一起等待浏览器定时重绘页面

#### 3.display:block

定义这个属性后，内联（非块状）元素也可以自己定义宽度和高度

display的值：none为不显示，block为显示为块级元素，inline为默认，显示为内联元素，Inline-block行内块元素（IE5.5将对象呈递为内联对象，但是对象的内容作为块呈递），run-in根据上下文选择是作为块级还是内联，list-item作为列表，table作为表格，其他同理，inherit从父元素那里继承display属性的值

#### 4.overflow属性

css属性overflow定义溢出元素内容去的内容会如何处理，如果值为scroll，不论是否需要，用户代理都会提供一种滚动机制，也就是必然出现滚动条

值为auto，子元素内容大于父元素时出现滚动条，visible，溢出元素出现在父元素之外，hidden，溢出部分隐藏

#### 5.position属性

static没有定位，元素出现在正常的流中，默认，忽略left,z-index等生命

relative生成相对于元素本身正常位置进行定位的元素，如left：20向元素的left位置（left坐标）添加20像素

absolute生成相对于static定位外第一个祖先元素进行绝对定位的元素

fixed生成相对于浏览器窗口绝对定位的元素

#### 6.counter

#### 7.布局错乱的问题

盒子模型，content padding border margin的关系

浮动布局时父元素是否设置了宽度和清除浮动，是否设置了position，清除浮动的原理

#### 8.两种盒子模型 box-sizing

CSS本质上是一个盒子，封装周围的HTML元素，包括边距，边框，填充，和实际内容  margin外边距 border边框 padding内边距 content内容

默认情况下，元素的实际宽度=width+padding+border+margin 会导致实际宽度比设置得大，而且这些值定义的是单边，计算要加上左右，比如padding:25px 实际要加50

box-sizing属性设置在元素的总宽度和高度中是否包含边距padding margin和边框border

默认属性值为content-box 意为设置的宽高是内容部分的宽高

设置box-sizing: border-box 使元素的内边距和边框在已设定的宽度和高度内绘制，也就是设定的宽高就是总宽高

还有一个值是inherit 也就是从父元素继承这个属性的值

#### 9.设置水平垂直居中的方法

水平居中：margin: 0 auto; //记得设置宽度（注意不要设行内元素，没用）

水平垂直居中：

a. position 已知宽度

父元素：position:relative; 

子元素：position:absolute  left:50% top:50% margin: -50px 0 0 -50px(减去自身宽度的一半)

b. position transform 未知宽度

将margin替换为transform: transalte(-50%, -50%)

c. flex布局

display: flex; justify-content: center;//水平居中 align-items: center;//垂直居中

d. table-cell布局

相当于表格的td，td为行内元素，无法设置宽和高，所以嵌套一层，设置inline-block

即父元素：display: table-cell vertical-align: middle//垂直 text-align: center//水平

子元素：display: inline-block width: height:

#### 10.flex弹性布局

设置的是弹性项目的弹性长度，如果元素非弹性则无效，是flex-grow flex-shrink flex-basis的简写

默认版本是 0 1 auto

flex-grow相对于其他弹性项目的增长量 

div:nth-of-type(1) { flex-grow:1;} div:nth-of-type(2){ flex-grow:3 ;}表示让第二个弹性项目的宽度是第一个的三倍

 flex-shrink收缩量 basis项目初始长度

flex-direction: row column row-reverse 

.flex-container{ display:flex; flex-direction: row;}

@media(max-width: 800px){ .flex-container{  flex-direction:column; }} //响应式布局，屏幕尺寸不同的时候展示方式不同

#### 11.position: sticky粘滞定位

可以理解为relative和fixed布局的混合

流盒：指粘性定位元素最近的可滚动元素（即overflow属性值不是visible)的尺寸盒子，如果没有就是浏览器视窗盒子

粘性约束矩形：粘性布局元素的父元素

粘性约束矩形在可视范围内为relative，反之为fixed

#### 12.BFC的触发条件

Block Format Context 块格式化上下文

是web页面的可视化css渲染的一部分，是布局过程中生成块级盒子的区域，一定是方形的，是独立的布局单元

创建方式、触发条件为

根元素或包含根元素的元素

float不是none，即浮动元素

绝对定位元素，position为absolute或fixed

display:inline-block 行内块元素 table-cell表格单元格 table-caption表格标题 flex弹性元素 grid网格元素

#### 13.position有哪些值

position属性规定应用于元素的定位方法的类型，共有五个值

static 静态 默认的，不受top left这些值的影响，始终根据页面的正常流进行定位

relative 相对于其正常位置进行定位 比如left: 30px 左边加30，即右移 

fixed 相对于视口定位，也就是说即使滚动页面，也始终处于同一位置

absolute 相对于最近的定位祖先元素（即除了static）进行定位，如果绝对定位的元素没有祖先，它将使用文档主体body，并随页面滚动一起滚动

sticky 粘性定位 根据用户滚动位置在relative和fixed之间切换，起初是relative，当视口偏移，即将离开视口的时候切换成fixed 粘贴在固定的位置，如向上滚动的时候他一碰到顶就停在那里了 top: 0；注意必须指定top bottom left right的至少一个

<h4>14.opacity透明度</h4>

opacity定义整个元素的透明度

fill-opacity 填充颜色的透明度 stroke-opacity 轮廓颜色的透明度 

合法范围是0-1

## JavaScript

#### 1.flash和js通过ExternalInterface类交互

该接口有两个方法，call和addCallback

ExternalInterface.call("js方法",传给js的参数) 在flash中调用js方法

ExternalInterface.addCallback("js flash",falsh) 注册flash函数来让js调用

#### 2.跨域及其实现方案

浏览器使用js跨域获取数据

a.js可以通过jsonp进行跨域

var script=document.createElement("script");

script.src="url?callback=handleResponse";

document.body.insertBefore(script,document.body.firstChild);

jsonp由回调函数和数据组成，回调函数是接收到响应时应该在页面中调用的函数，其名字一般在请求中指定，数据是传入回调函数的json数据。优点是能够直接访问响应文本，可用于浏览器和服务器之间的双向通信，缺点是jsonp从其他域中加载代码执行，其他域未必安全，还有难以确定jsonp请求是否失败

b.使用window.name进行跨域

c.通过修改document.domain来跨子域

将页面的document.domain设置为相同的值，页面间可以互相访问对方的js对象，注意不能将值设为url中不包含的域，松散的域名不能再设置为紧绷的域名

d.CORS Corss-Origin Resource Sharing 跨资源共享

基本思想是使用自定义的HTTP头部来让浏览器和服务器进行沟通，从而决定请求或响应的成败，即给请求附加一个额外的origin头部，其中包含请求页面的源信息（协议，域名，端口），服务器根据这个头部决定是否响应。所以，只要协议(http https)、端口(:80 :8080)、域名有一个不同，就是不同的域

e. Comet

Comet可实现服务器向浏览器推送数据,实现方式是长轮询和流

短轮询即浏览器定时向服务器发送请求，看有没有数据更新。长轮询即浏览器向服务器发送一个请求，然后服务器一直保持连接打开，直到有数据可发送。发送完数据后，浏览器关闭连接，随即又向服务器发起一个新请求。其优点是所有浏览器都支持，使用XHR对象和setTimeout()即可实现

流即浏览器向服务器发送一个请求，而服务器保持连接打开，然后周期性地向浏览器发送数据，页面的整个生命周期内只使用一个HTTP连接。

f. WebSocket

WebSocket可在一个单独的持久连接上提供全双工、双向通信。

WebSocket使用自定义协议，未加密的连接时ws://；加密的链接是wss://。

var webSocket=new WebSocket("ws://");

webSocket.send(message);

webSocket.onmessage=function(event){

var data=event.data;...}

注意：必须给WebSocket构造函数传入绝对URL；WebSocket可以打开任何站点的连接，是否会与某个域中的页面通信，完全取决于服务器；WebSocket只能发送纯文本数据，对于复杂的数据结构，在发送之前必须进行序列化JSON.stringify(message))。

优点：在客户端和服务器之间发送非常少的数据，减少字节开销。

g. 图像ping

var img=new Image();

img.onload=img.onerror=function(){... ...}

img.src="url?name=value";

请求数据通过查询字符串的形式发送，响应可以是任意内容，通常是像素图或204响应。

图像Ping最常用于跟踪用户点击页面或动态广告曝光次数。

缺点：只能发送GET请求；无法访问服务器的响应文本，只能用于浏览器与服务器间的单向通信。

#### 3.作用域和闭包

js变量属于本地或全局作用域

全局变量可以通过闭包实现局部化

不通过var创建的变量总是全局的，即使它们在函数中创建

为了限制一个变量是全局但只被一个函数修改，使用嵌套函数

function add(){

  var counter = 0;

  function plus() { counter++; }

  plus();

  return counter;

}

闭包指一种保护私有变量的机制，在函数执行时形成私有的作用域，保护里面的私有变量不受外界干扰。直观的说就是形成一个不销毁的栈环境。

function Foo() {var i = ``0``;return function() {console.log(i++);}}

``var f1 = Foo(),``  f2 = Foo();

f1();``f1();``f2();

foo函数返回了一个匿名函数的引用，即一个闭包，它可以访问到foo()被调用的环境，而局部变量i一直早这个环境中，只要一个环境还有可能被访问到，局部变量就不会销毁，所以闭包有延续变量作用域的功能

第一次f1() i=0 第二次f1() i=1

但因为foo()返回的是匿名函数，所以f1 f2指向的是两个不同的函数对象，所以f2() i=0

function是引用类型，保存在堆中，f1 f2保存在栈中

#### 4.用js改变元素样式

让一个input的背景颜色变成红色：inputElement.style.backgroundColor="red";

获取元素可以借助document.getElementById()或getElementByTagName()等方法，也可以借助层级关系

通过js来改变元素样式最常见的两个API是style和className，使用style接口一次只能改变一个样式，而使用className则可以同时改变多个样式，前提是已经用css定义过（只能改变不能现定义）

ps:表示红色：red rgb(100%,0%,0%) rgb(255,0,0) #FF0000 #F00

#### 5.关于数据类型

数据类型：数值，字符串，布尔值，数组，对象

对象用花括号书写，属性是name: value对，用逗号分隔，如 var x={ firstName:"Bill", lastName:"Gates"}

当数值和字符串相加时，js把数值视作字符串

js从左向右计算表达式

911+7+"abc" 结果为"918abc" "abc"+911+7 结果为"abc9117"

js是动态类型，也就是说同一个变量可以被赋予不同类型的值

typeof运算符可以返回变量或表达式的类型，如 typeof "bill" 返回"string" typeof 0 返回"number" 对数组返回"object" 对函数返回"function"

没有值的变量，其值和类型均为undefined 任何变量均可通过设置为undefined进行清空

空值""是字符串，null是对象

null与undefined值相等但类型不相等，所以==true ===false

原始数据是指没有额外属性和方法的单一数据值，复杂数据则是object或function

#### 6.原型链

在js中，每个函数都有一个prototype属性，指向函数的原型对象

function Person(age) { this.age=age;}

Person.prototype.name="abc"

console.log(new Person().name) //abc

每个js对象（除Null)创建的时候都会关联原型对象，从中继承属性，还有一个 _ proto_属性会指向该对象的原型

new Person()._ proto_===Person.prototype

不过实际上_ proto_是obj.getPrototype(obj)

读取实例的属性，如果找不到就回去找原型，原型的原型，直到找到最顶层为止

每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。那么假如我们让原型对象等于另一个类型的实例，结果会怎样？显然，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立。如此层层递进，就构成了实例与原型的链条。这就是所谓的原型链的基本概念。

![img](https://img2018.cnblogs.com/blog/850375/201907/850375-20190708153139577-2105652554.png)

#### 7.var let const的区别

通过var定义的变量没有块作用域，在块{}内声明的变量快外依然可以访问

let声明拥有块作用域的变量，即{}之外无法访问

在函数内声明则都属于函数作用域，函数外则是全局作用域

HTML中全局作用域是window对象，var定义的全局变量就属于window对象，即var name 可以使用window.name而let不行

var允许重新声明 即var x = 10 var x = 6 可行

而let x = 10 let x = 6 不可行

let x = 10 var x = 6或交换顺序也是不行的

在不同的块中重新声明let是允许的

通过var声明的变量会提升到顶端，即在声明之前就可以使用，let和const不行

const必须在声明时赋值且值不可以改变

常量对象的属性可以更改如const car = { type:"a”}

可以修改属性如car.type="b" 可以增加属性如car.name="bmw" 但不能car={ type:"b"}

同理，常量数组的元素可以更改和增加，但不能直接赋值

在不同的作用域或块中重新声明const是允许的，如const x = 2 { const x = 3}

局部变量必须通过var关键词来声明，否则会成为全局变量

#### 8.修改this的指向

#### 9.防抖节流

#### 10.变量提升

#### 11.promise any

#### 12.箭头函数

 (x)=>x (x)=>(x) (x)=>({x}) 输入x=1 输出分别是什么

(x) =>(x)   1
(x)=>{(x)}  undefined
(x) =>({x})  {x:1}

#### 13.一些基础

##### 脚本文件

把脚本放在外部文件中：<script src=".js"> 注意外部脚本无需再包含<script>标签

放在内部的话head或body都行，但放在body的底部，可以改善显示速度，因为脚本编译会拖慢显示

##### 显示

window.alert() 写入警告框 

document.write() 写入HTML输出  调用时会清除所有已有的HTML，即删光页面只剩该输出，所以只用于测试

innerHTML 写入HTML元素 使用document.getElementById(id)

console.log() 写入浏览器控制台 在浏览器的F12控制台里

##### 语法

js中不使用连字符‘ -’，会判断为减法

可以在一条语句中声明并赋值多个变量 var a=1, b='abc',c=2.0;

声明还可以横跨多行 var a=1,

b='abc',

c=2.0;

var name; // 这条语句执行后name的值是undefined

重复声明值不会丢，如var name='ab'; var name; 则name的值仍旧是ab

注意"8"+3+5结果为835 3+5+"8"结果为88 因为js是从左往右编译

位运算中使用32位有符号数，>>为有符号右移 >>>为零填充右移 ^按位异或

%返回余数，即取模

**取幂，5 * * 2即为25 等价于Math.pow(5, 2)

运算符in表示对象中的属性，如"PI" in Math

运算符instanceof表示对象的实例，如instanceof Array

##### 类型

js是动态类型

typeof返回变量的类型

instanceof返回该对象是否是对象类型的实例，是则返回true

数组 ["a","b","c"]   typeof数组返回"object" 没有值的变量返回undefined

对象{ firstname:"a", lastname:"b", age:25, eyecolor:"blue"}

var p = null;//  值是null 类型仍然是对象 typeof null返回"object"

var p = undefined;//  值和类型都是undefined 彻底清空

typeof function myFunc() 返回"function"

##### 常见的HTML事件

onchange HTML元素已经被改变

onclick点击 onkeydown按下键盘按键

onmouseover鼠标移动到元素上 onmouseout鼠标从元素上移开

onload 浏览器已经完成页面加载

##### 对象

```
document.getElementById("demo").innerHTML = toCelsius(77)
```

innerHTML = toCelsius ; 则加入的是函数的定义

访问对象属性可以person.firstname 也可以person["firstname"]

##### 字符串操作

str.length返回字符串长度

对'' "" \在前面使用转义字符\

使用 var x = new String() 声明的是对象

var x = new String("bill") var y = new String("bill") x==y就为false因为不同的对象无法比较

str.indexOf()返回指定文本在字符串中首次出现的索引，一个字符串的话返回首字母的索引

lastIndexOf()返回最后一次出现的索引，仍然是首字母

如果没找到的话均返回-1

两种方法都接受第二个参数作为检索的起始位置，但indexOf("ab",50)是从50开始往后找，lastIndexOf(..50)是从50开始往前找

而search()方法无法设置第二个参数，但比indexof支持正则表达式

提取字符串

slice(start,end) 参数是起始和终止的索引，返回被提取的部分，如果是负数则倒着数，如果省略第二个参数则直接提取到剩余部分

substring(start,end) 与slice相似但不能接收负的索引

substr(start,length) 第二个参数是被提取部分的长度 长度不能为负数但索引可以

var str1 = "abcde"

var str2 = str1.replace("a","x") 不会改变str1的值，返回的是新字符串，默认只替换首个匹配，大小写敏感

如果想大小写不敏感使用正则/a/i不带引号

如果想替换所有匹配使用 /a/g

转换为大写str1.toUpperCase()

小写str1.toLowerCase()

var str1 = "hello"

var str2 = str1.concat(" ","world") 等效于"hello"+" "+"world"

所有字符串方法都会返回新字符串，不修改原有的

str1.trim() 删除两端的空白符

str1.charAt(0) 返回指定下标的字符 找不到则返回空字符串“”

str1.charCodeAt(0) 返回指定下标字符的unicode编码 如H对应72

支持str1[0] 但是注意他找不到返回undefined，而且是只读的 str[0]="a" 不会报错但也不会工作

使用split()把字符串转换为数组

var str="a,b,c,d,e"

str.split(",") // a b c d e

str.split("") // a , b , c , d , e

##### 数字

数值始终是64位浮点数 即双精度 0-51位存数字，52-62存指数，63存符号

0.2 + 0.1 会得到0.30000000000000004

解决方法是 ( 0.2 * 10 + 0.1 * 10 ) / 10

注意只有+会在字符串上作用为级联

"100" / "10" = 10 乘法减法也都可以，只有加法不行

100 / "10" = 10 但100 / "apple" 结果为NaN 即Not a Number isNaN()会返回true typeof NaN 返回"number"

Infinity 最大值

0开头八进制，0x开头十六进制

使用toString() 把数字输出为其他进制，如var num=128

num.toString(16) 得80 num.toString(2)得1000000

toString() 以字面量、变量或表达式返回数字，如(100 + 23).toString() 返回123

toExponential()返回四舍五入并使用指数计数法的数字，参数是小数点后的位数

var x = 9.656 x.toExponential(4) 返回9.6560e+0

toFixed() 返回指定位数小数的数字 x.toFixed(2) 返回9.66

toPrecision() 返回指定长度字符串 x.toPrecision(2)返回9.7

##### 将变量转换为数字的全局方法

Number() 不能转换返回NaN 把日期如new Date("2019-04-15")返回1970年1月1日至今的毫秒数

parseFloat() 空格只返回首个数字，如"10 20 30"返回10

parseInt() ”10 20 30“返回10 ”10.33“也返回10

Number.MAX_VALUE 最大数字 MIN_VALUE POSITIVE_INFINITY NEGATIVE_INFINITY溢出时返回

这些数字属性属于名为number的js数字对象包装器，只能用Number访问，用其他访问返回undefined

##### 数组

var array = ["..."]

元素数量array.length 

排序array.sort()

访问元素array[array.length-1]

遍历 for(var i = 0;i<array.length;i++)

添加元素 array.push("newitem") 或array[array.length]="newitem"

不支持命名索引

构造的时候避免使用new Array() 可以使用[] var points=[ ] var points=[1,2,3]

如何判断某个变量是否是数组

Array.isArray()

function isArray(x){

  return x.constructor.toString(),indexOf("Array") >-1; // 即对象原型包含单词”array“

}

x instanceof Array 返回true

赋值表达式返回所赋的值，所以var x = 0 if ( x = 0 ) 为false

switch语句使用的是严格比较 var x = 10 switch(x) {  case 10: dui; case "10": cuo; }

```
for (var i = 0; i < 10; i++) {
  // 代码块
}
return i;
```

得到的结果是10

##### JSON

JSON是存储和传输数据的一种轻量级数据交换格式，JavaScript Object Notation 语法来自js对象符号但格式是纯文本，读取和生成JSON数据的代码可以在任何编程语言中编写，所以独立于语言

例如

{

"employees":[

  {"firstName" : "bill" , "lastName" : "Gates"},

  {"firstName" : "steve" , "lastName" : "Jobs"}

]

}

数据是名称、值的键值对，用逗号分隔，花括号保存对象，方括号保存数组

可以先把JSON转换为js字符串 vat text = ...

. 然后把字符串转换为js对象 var obj = JSON.parse(text)

#### 14.Web Worker

在HTML页面中，如果在执行脚本时，页面的状态是不可响应的，直到脚本执行完成后，页面才变成可响应。web worker是运行在后台的js，独立于其他脚本，不会影响页面你的性能。并且通过postMessage将结果回传到主线程。这样在进行复杂操作的时候，就不会阻塞主线程了。

如何创建web worker：

检测浏览器对于web worker的支持性

创建web worker文件（js，回传函数等）

创建web worker对象

## HTTP

#### 1.计算机网络的七层模型OSI

应用层：文件传输，HTTP SNMP,FTP

表示层：数据格式化，代码转换，数据加密

会话层：建立和解除会话

传输层：提供端对端的接口 TCP UDP

网络层：为数据包选择路由 IP ICMP

数据链路层：传输有地址的帧

物理层：在物理媒体上传输二进制数据

p.s.TCP/IP的四层是链路，网络，传输，应用

#### 2.TCP和UDP的区别

a.TCP是面向连接的，UDP无连接，即发送数据前不需要先建立连接

b.TCP提供可靠的服务，即通过TCP传送的数据无差错不丢失不重复且按序到达，而UDP尽最大努力交付，不可靠

c.TCP面向字节流，UDP面向报文，且网络拥塞不会导致发送速率降低，因此会出现丢包

d.TCP只能一对一，UDP支持一对多

e.TCP的首部较大为20字节，UDP只有8字节

#### 3.HTTP HTTPS的区别 SSL

##### **定义：**

HTTP 超文本传输协议，使用TCP从万维网服务器传输超文本到本地浏览器的传输协议，可以使浏览器更加高效，网络传输减少

HTTPS  HyperText Transfer Protocol over Secure Socket Layer是HTTP加入SSL层构建的可进行加密传输和身份验证的网络协议，安全性更高

SSL Secure Sockets Layer 安全套接层 提供私密性、信息完整性和身份认证，不依赖平台和应用程序

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019051117402024.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjY1MTM3,size_16,color_FFFFFF,t_70)

SSL包含两个协议子层
1）底层：SSL记录协议层 SSL Record Protocol Layer 记录封装各种高层协议，具体实施压缩解压缩，加密解密、计算和校验MAC等与安全有关的操作

2）高层：SSL握手协议层 SSL HandShake Protocol Layer 握手协议，密码参数修改协议，警告协议，用于管理信息的交换，允许协议传送数据相互验证、协商加密算法和生成密钥等，作用是协调客户和服务器的状态，使双方能够达到状态的同步

##### **区别：**

a.HTTP传输的数据都是明文（未加密）的，而HTTPS是具有安全性的SSL加密传输协议

b.使用不同的链接方式，端口也不同，HTTP为80，HTTPS为443

c.HTTPS握手阶段比较费时，增加耗电和数据开销，SSL证书需要绑定IP，需要ca证书，非要较高

d.HTTP的连接比较简单，是无状态的，而HTTPS是可加密传输、身份认证的

##### **HTTPS的工作原理**

客户使用HTTPS URL访问服务器，则要求服务器建立SSL链接，服务器收到请求后将网站的证书（其中包含公钥）返回给客户端，双方协商SSL链接的安全等级，也就是加密等级，协商一致后客户端浏览器根据该等级建立会话密钥，然后根据网站的公钥来加密会话密钥，并传送给网站，服务器通过自己的私钥解密出会话密钥，然后通过会话密钥加密通信

HTTPS的优缺点

1.现行架构下最安全的方案，虽不是绝对安全但大幅增加了中间人攻击的成本

2.确保数据发送到正确的机器上，防止数据在传输中被窃取、改变

3.握手费时，缓存也不如HTTP高效，增加加载时间、耗电、数据开销，SSL证书也需要费用

4.SSL证书需要绑定IP，不能在同一个IP上绑定多个域名，而IPV4的资源支持不了这种消耗

#### 4.HTTP 1.X 2.0的区别

HTTP2.0是基于1.0的更新，不是重写，方法、状态、api在2.0里是一样的

2.0协议重点是对终端用户的感知延迟、网络及服务器资源的使用等性能优化

1.内容安全：因为基于HTTPS

2.提升了访问速度：请求资源所需时间更少

3.允许多路复用/连接共享：原本1.x在同一时间、针对同一域名的请求由一定数量限制，超过该数目的请求会被阻塞，所以一些站点会有多个静态资源域名。而2.0允许同时通过单一链接发送多重请求-响应信息，每个数据流被拆分成很多互不依赖的帧，这些帧可以交错发送，还可以分优先级，到另一端再组合起来。即每个request请求可以随机地混杂在一起，接收方根据request id将其归属到不同的请求里。多路复用支持了流的优先级，允许客户端告诉服务器哪些内容优先传输 优先级：主要html>css>js>图片

4.二进制分帧：HTTP1.x的解析是基于文本的，而2.0将所有传输信息分割为更小的消息message（一个完整的请求或响应，由若干frame组成）或帧frame（类型，长度，flags，流标识，有效载荷），并对他们进行二进制编码，使协议有更多可扩展性，比如引入帧来传输数据和指令

5.首部压缩 Header Compressing 1.x的头部带有大量信息且每次都要重复发送，2.0使用encoder来减少需要传输的头部大小，双方各自缓存一份头部字段表，对于相同的数据不再通过每次请求和响应发送，通信期间几乎不会改变的通用键值对（如用户代理，可接受的媒体类型等）只需发生一次 首部表在2.0的连续存续期内始终存在，由双方共同渐进更新

6.服务器端推送

#### 5.IPV4 IPV6区别

#### 6.三次握手四次挥手

#### 7.GET POST请求的区别

#### 8.WebSocket的实现和应用

WebSocket是HTML5的协议，支持持久连续的连接，而HTTP协议不支持

#### 9.常见状态码

400：请求无效

产生原因：1.前端提交数据的字段名称和字段类型与后台的实体没有保持一致

2.前端提交到后台的数据应该是json字符串类型，但是前端没有将对象JSON.stringify转化成字符串。

解决方法：1.对照字段的名称，保持一致性

2.将obj对象通过JSON.stringify实现序列化

401：当前请求需要用户验证

403：服务器得到请求但拒绝指向

#### 10.fetch发两次post请求的原因

fetch发送post请求的时候，总是发送2次，第一次状态码是204，第二次才成功？

原因很简单，因为你用fetch的post请求的时候，导致fetch 第一次发送了一个Options请求，询问服务器是否支持修改的请求头，如果服务器支持，则在第二次中发送真正的请求。

## Vue

#### 1.在Vue中渲染一个页面的过程

1.解析语法生成AST

2.根据AST结果完成data数据初始化

3.根据AST结果和data数据绑定情况，生成虚拟DOM

4.将虚拟DOM生成真正的DOM插入到页面中，此时页面被渲染

更新数据时

5.框架接收数据变更的事件，根据数据生成新的虚拟DOM

6.比较新旧两个，得到差异

7.根据差异更新页面内容

清空页面内容

8.注销实例，清空页面内容，移除绑定事件、监听器等

#### 2.生命周期钩子

![image-20210708145305288](C:\Users\rickyrqzhao\AppData\Roaming\Typora\typora-user-images\image-20210708145305288.png)

#### 3.Vue中的methods

如果说数据是状态机，那事件就是状态机的扭转

比如列表

export default {

  data(){

​    return {  tableData: tableData } //列表数据

  }，

  methods: {

​    add/delete/moveTableItem

  }

}

#### 4.组件是什么

1.组件内维护自身的数据或状态  对应vue中的 data

2.组件内维护自身的事件 methods 生命周期钩子

3.通过提供配置方式来控制展示，或者控制执行逻辑 props

4.通过事件触发、监听或API提供，提供与外界（如父组件）通信的方式 $emit $on

在页面中抽象和划分组件，通常使用所见即所得的方式，如“菜单”，“表格”

.vue是一个单文件组件，就是把html css js写在一个文件里，分别用<template> <style> <script>来分隔 不过script style也可以通过src来引入

#### 5.组件的data

一个组件的data选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝

如果不是组件的话正常data可以直接写成对象，如data: { msg: 'load' } 但由于组件会在多个地方引用，js直接共享对象会造成引用传递，也就是说修改了msg后所有实例的msg都会跟着改，所以这里要用函数来返回对象实例

#### 6.绑定数据、事件、表单

1.数据双向绑定

双大括号绑定普通文本 {{  msg  }} 只能插入到节点的内容中

双大括号可以使用js表达式 {{  msg.split(' ').reverse().join('')  }}

v-html输出的内容不会被模板引擎过滤

v-bind:id="a" 绑定属性 缩写为: 也可以用来传参

父子组件之间的数据传递，通常通过props和事件进行传递，父组件通过props绑定数据到子组件，通过事件监听获取子组件的数据更新

或者使用vuex rxjs这种状态管理的工具

2.事件绑定

v-on:click="counter+=1" @click="function"

3.表单绑定

v-model绑定表单的值

<input v-model="val">等同于<input :value="val" @input="update />

#### 7.文件结构

package.json保存一些依赖信息，config保存项目初始化配置，build里面是webpack初始配置，build目录中webpack.base.conf.js中

module.exports = {

  entry: {  app: './src/main.js' } ,//说明入口js文件是main.js

  output: ...

}

![image-20210708154929857](C:\Users\rickyrqzhao\AppData\Roaming\Typora\typora-user-images\image-20210708154929857.png)

pages文件夹存放页面，components文件夹存放具体组件，包括按钮，箭头，列表，导航等

#### 8.配置路由

传统的php路由是服务器端根据一定的url规则匹配来给前端返回不同的页面代码

可以使用锚点路由来实现不需要刷新页面的简单路由 #

<router-view></router-view>在页面中放入一个路由视图容器

配置路由：

引入路由插件vue-router

显示声明要用的路由 Vue.use(Router)

export default new Router({ //注册路由器

  routes:  [ //开始配置

 {   path:  '/' , name: 'hello' , component: hello  },

  {  path: '/about', name: 'about', component: hello } //他对应的路由地址是http://localhost:8080/#/about

  ]

})

#### 9.如何实现视图更新

#### 10.watch和computed的关系

#### 11.data不更新时 computed还会缓存、执行吗

#### 12.服务器端是如何解析vue组件进行渲染的，和客户端渲染有何不同

#### 13.v-for key

14.两种路由模式 history 怎么体现hash值得变化

15.v-if v-show

16.组件间通信

## React

#### 1.页面渲染流程

#### 2.性能优化

## jQuery

## Webpack

#### 1.概念和原理

是一个前端资源加载、打包的工具，根据模块的依赖关系进行静态分析，按照指定的规则生成对应的静态资源

![img](https://www.runoob.com/wp-content/uploads/2017/01/32af52ff9594b121517ecdd932644da4.png)

譬如图中将多个js css合成一个

其本身只能处理js模块，要处理其他文件需要使用loader进行转换，如css-loader遍历css文件，找到url()表达式然后处理他们，style-loader把原来的css代码插入页面的style标签中

#### 2.有什么优点

## Element-ui

## Redux

#### 1.工作流程