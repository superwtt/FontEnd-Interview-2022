#### 从输入url开始，到页面呈现，经历了哪些过程

1. 输入地址
2. 浏览器查找域名的IP地址
3. 浏览器向web服务器发送HTTP请求
4. 服务器永久重定向响应
5. 服务器处理请求
6. 服务器返回一个HTTP响应
7. 浏览器显示HTML
8. 浏览器发送请求获取HTML中的资源（图片、css、js、视频音频等等）

---

##### <font color=Firebrick1 >1. 输入地址</font>

当我们开始在浏览器中输入网址的时候，浏览器其实已经在智能地匹配可能的url了，它会从历史记录、书签等地方找到已经匹配过的url，然后给出智能提示，让我们可以补全url地址

---

##### <font color=Firebrick1 >2. 浏览器查找域名下的IP地址</font>

1. 请求一旦发起，浏览器首先要做的事情就是解析这个域名，一般来说，浏览器会首先查看本地硬盘的hosts文件，看看其中有没有和这个域名对应的规则，如果有的话就直接使用hosts文件里面的ip地址

2. 如果没有，则浏览器会发出一个DNS请求到本地DNS服务器，DNS是域名系统。查询流程如下：

   + 本地DNS系统

   + DNS根服务器系统

   + .com/.cn域服务器

   + 域名解析服务器系统

   + 收到一个域名和IP地址的对应关系

   + 本地DNS将这个对应关系保存在缓存中，以备下次访问时可以直接返回结果

     ![](https://pic3.zhimg.com/80/v2-367da995706289a83af5c0372d55f43e_720w.jpg)

---

##### <font color=Firebrick1 >3. 浏览器向web服务器发送一个HTTP请求</font>

拿到域名对应的IP地址后，浏览器会向服务器发起一个TCP的连接请求。这个请求到达服务器之后，进入到网卡，然后进入到内核的TCP/IPC协议栈，最终到达web程序，建立起TCP/IP连接，发送HTTP请求

---

##### <font color=Firebrick1 >4. 服务器的永久重定向响应</font>

---

##### <font color=Firebrick1 >5. 浏览器跟踪重定向地址</font>

---

##### <font color=Firebrick1 >6. 服务器处理请求</font>

---

##### <font color=Firebrick1 >7. 服务器返回一个HTTP响应</font>

---

##### <font color=Firebrick1 >8. 浏览器显示HTML</font>

浏览器是如何把页面呈现在屏幕上的呢？不同浏览器的解析过程不一样，这边我们只了解 webkit 的渲染过程，这个过程包括：

> 解析html以构建dom树 -> 构建render树 -> 布局render树 ->绘制render树



忽略网络请求的部分，从地址栏输入url到页面加载完成，大概经历了以下几个过程：

+ <font color=Orange>首先浏览器自上而下逐行解析HTML内容，经过词法分析、语法分析构建DOM树</font>
+ <font color=Blue>当遇到外部CSS链接时，主线程调用网络请求异步获取资源，不阻塞进程继续构建DOM树。当CSS下载完成之后，解析CSS文件构建CSSOM树</font>
+ <font color=Khaki3>浏览器结合DOM树和CSSOM树构建render树</font>
+ <font color=IndianRed1>渲染树构造完成之后，浏览器开始布局渲染树并将其绘制到屏幕上。这个时候涉及到两个概念：reflow回流（重排）和repaint（重绘）</font>
+ <font color=Tan2>当遇到外部JS链接时，主线程调用网络请求模块异步获取资源，由于JS可能会修改DOM树和CSSOM树而造成回流或者重绘，所以此时的DOM树构建出于阻塞状态</font>
+ <font color=Red>当JS下载完成之后，浏览器调用V8引擎解析、编译js、并在主线程中执行</font>
+ <font color=BlueViolet>解析JS，JS的解析是由浏览器中的JS解析引擎完成的。JS是单线程的，同一个时间只能执行一件事情，所有的任务都需要排队，前一个任务完成之后，才能继续下面的任务。某些比较耗时的任务，放在异步队列里面，主线程和异步队列同时进行，当异步队列有结果了，继而会放到事件队列里面，当主线程空闲了，会去事件队列里面读取事件进行执行，执行完之后继续去读取，这个过程是不断重复的，所以又叫事件循环</font>
+ <font color=Seashell4>**浏览器处理JS执行流程大致如上，但细分到每段JS的具体执行流程如下**</font>
+ <font color=Blue>JavaScript引擎并非一行一行的分析和执行程序的，而是一段一段的分析执行。当执行<code><font color=red>一段代码</font></code>的时候，会进行一个“准备工作</font>
+ 一段代码（可执行代码）分为三种：
    + 全局代码
    + 函数代码
    + Eval代码
    
+ 即 执行上下文的创建

+ <font color=Seashell4>当执行流进入某个执行环境时，先将全局上下文压入执行栈，当调用某个函数时，再将这个函数压入执行栈。其中，在调用函数之后，执行函数之前，此函数会做一些“准备工作”，即变量对象的初始化，包括函数的形参，函数声明，变量声明，随着函数的执行，这边变量对象的属性会分别被赋值。</font>

+ <font color=MediumPurple1>当函数执行完成之后，JS引擎会将该函数从执行栈中弹出，接着执行下一个环境，当全部程序执行完成之后，将全局上下文从执行栈中弹出，程序结束</font>

---

1. 浏览器将url解析成ip地址
2. 浏览器与目标服务器发起TCP连接
3. 浏览器通过http协议向服务器发起请求
4. 服务器响应http请求，浏览器得到html代码
5. 浏览器解析html代码，并请求html中的资源
6. 浏览器渲染页面呈现给用户