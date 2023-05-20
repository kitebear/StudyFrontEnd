JSBridge 是 JavaScript 和 Native 相互通信的一种方案。它的实现流程一般如下：

1. 在 Native 端开启一个 Webview，并让其加载一个 HTML 页面。

2. HTML 页面中通过 JavaScript 调用 Native 端提供的接口，例如获取设备信息、选择图片等。

3. 在 Native 端，为 Webview 设置一个 JavaScriptInterface 对象，即使这个对象可以被 JavaScript 代码调用。

4. 在 Javascript 代码中，通过 window[objName].[methodName] 来调用 Native 提供的方法。

5. JSBridge 提供了约定好的协议，来格式化 Native 端和 Javascript 端传递的数据，例如通过 JSON 格式来实现数据的传递和解析。

6. Native 端通过 WebViewClient 或者 WebChromeClient 的回调方法将数据传递给 Javascript 端，Javascript 端通过约定好的协议格式来解析数据。

总的来说，JSBridge 的实现主要是通过 Javascript 和 Native 双方开放接口并约定传输协议，来实现双方的通信。其优势在于解决了 JavaScript 和 Native 交互的问题，能够实现许多 Hybrid App 中的功能。






webView   onMessage injectedJavascript

H5 -> Native    postMessage
Native -> H5    拼接injectedJavascript参数去调用的