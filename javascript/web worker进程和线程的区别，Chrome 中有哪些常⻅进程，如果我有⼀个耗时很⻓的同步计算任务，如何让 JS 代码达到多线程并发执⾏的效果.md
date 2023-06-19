进程（Process）和线程（Thread）都是操作系统中用于多任务处理的概念。简单地说，一个进程就是一个程序的执行空间，而一个线程则是在执行空间内独立运行的执行路径。

区别：

1. 进程是系统分配资源的最小单位，线程是操作系统调度的最小单位。
2. 各个进程之间是独立的，各个线程之间共享一些资源。
3. 创建或销毁进程时，会涉及到比较多的系统资源，开销比较大，而线程的创建和销毁的开销相对较小。
4. 进程与进程之间会有 IPC（进程间通信）的开销，而线程之间的 IPC 开销较小。

在 Chrome 浏览器中，会有多个进程同时运行：

1. Browser 进程：负责管理整个浏览器的生命周期、显示窗口、处理用户操作等。
2. Renderer 进程：负责一个 Tab 内关联的页面的渲染和运行，每个 Tab 都有一个独立的 Renderer 进程。
3. GPU 进程：负责处理页面中使用的 WebGL 等 GPU 相关任务。
4. Plugin 进程：负责运行插件，包括 Flash 等。
5. Utility 进程：负责处理部分浏览器内部的任务，比如下载、DNS 等。

如果想让 JS 代码达到多线程并发执行的效果，可以使用 Web Workers。Web Workers 是运行在后台的 JavaScript 线程，可以让页面在主线程中执行 JS 代码的同时，创建一个或多个新的线程在后台执行耗时的任务。Web Workers 的 API 包括：

1. Worker 构造函数：用于创建一个新的 Worker 线程。
2. postMessage() 方法：用于向 Worker 线程发送数据。
3. onmessage 事件：用于监听 Worker 线程发送回来的消息。

举个例子，可以使用如下代码创建一个 Worker 线程：

```js
// file: worker.js
self.addEventListener('message', function(e) {
  var data = e.data;
  self.postMessage(data);
}, false);

// file: main.js
var worker = new Worker('worker.js');
worker.addEventListener('message', function(e) {
  console.log('Worker said: ', e.data);
}, false);
worker.postMessage('Hello World');
```

在上面的例子中，我们在主线程中创建了一个 Worker 线程，向它发送了一条消息，并在 Worker 线程中处理这条消息，并将结果发回到主线程。这样就实现了多线程的并发执行效果。