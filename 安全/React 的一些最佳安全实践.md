[React 的一些最佳安全实践](https://mp.weixin.qq.com/s/TE6Tn1w1jb9TrrPy94OLAQ?st=7082F9971C84F7AA3EDE43D5753F9EA36B9719C2B77E50D153236469B2254A99D13F304163BA268289FD7B93F110985867DEFF076E63A45048771C0C5D031027A310BBECC42D2D7D2B7A00297ABF60DF40E76C0676FDCA834E22897A5A06D00416B2A7F921B987B552ECE468D3B9920518284C2085D47D97E43458F0FA915593BC5E26EEA332852B6D692D36D23BE0D3EF84E4EEB7517530A8BE932BB5CD89D528DCD5944F717E557B28A75CD2B9C00A8B0FCF0A0EF1436C407C652D118D93E94A78B4CC18785D15E60A609B168E47AA24748ED00FC231ECFFD23A360176DDFC568498C027B24AC456D867F5AB1FCC91&vid=1688852885958703&cst=2E70871D937BD4F55C13F545D17B0C8DA745F5CBA18D441D7508B301A4704658DBD477B7A2AD04B9EA34E19107B6766D&deviceid=86c8f513-9941-43fb-af94-f482653a36d9&version=4.0.8.99136&platform=mac)

`React.js、Vue.js` 这些现代的前端框架默认已经对安全做了非常多的考虑，但是这仍然不能阻碍我们写出一些安全漏洞 。。。因为框架永远不能完全限制我们编程的灵活性，只要有一定的灵活性存在就意味着有安全风险。

下面我就带大家一起来看一下，为了保证我们 React 应用的安全性，有哪些值得遵循的最佳实践。

## dangerouslySetInnerHTML

`React` 会对默认的数据绑定(`{}`)进行自动转义来防止 `XSS` 攻击，所有数据都会认为是 `textContent`：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e5Dzv8p9XdSdRkhk04cqxRQrq5OOtkick0kxcCwoZNOCCGAh16lLKV5QibIIPE5GN0a3sXRUa1gQMLibXa7h1fZ9Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

但是为了保障开发的灵活性，它也给我们提供了一些直接渲染 `HTML` 的方法，比如 `dangerouslySetInnerHTML`：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e5Dzv8p9XdSdRkhk04cqxRQrq5OOtkickTgnuiaoSfcHHUjsDMVRFibpyv5bdTjz4NXlM5V2GU20MAedhb8lVVjlA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在把数据传入 `dangerouslySetInnerHTML` 之前，一定要确保数据是经过过滤或转义的，比如可以通过 `dompurify.sanitize` 进行过滤：

`import dompurify from "dompurify";   import "./styles.css";      export default function App() {     const code = "<input onfocus=alert(1) autofocus />";     return (       <div className="App">         <div dangerouslySetInnerHTML={{ __html: dompurify.sanitize(code) }} />       </div>     );   }   `

## 避免直接操作 DOM 注入 HTML

除了 `dangerouslySetInnerHTML` ，我们当然还可以直接通过原生的 `DOM API` 来插入 `HTML`：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e5Dzv8p9XdSdRkhk04cqxRQrq5OOtkickDgVFVtLvsgsia8oSO7nEaSmiaopOygNS3zhloNOMokHgPut8QSicBoeag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

另外也可以通过 `ref` 来访问 `DOM` 来插入 `HTML`：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e5Dzv8p9XdSdRkhk04cqxRQrq5OOtkickaP0jtRxd3tEVZcUJeqQiaenxcEFtZbMfd3BnQTasagTibRibDbYVibjR0g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这两个操作都是相当危险的操作，推荐大家既然用了 `React` 就要尽量用 `React` 的编写方式来写代码，尽量不要直接操作 `DOM`，如果你确实要渲染富文本，还是推荐用上面提到的 `dangerouslySetInnerHTML`，而且数据要经过过滤或转义。

## 服务端渲染

当使用服务端渲染函数时，数据绑定也会提供自动内容转义，比如 `ReactDOMServer.renderToString()` 和 `ReactDOMServer.renderToStaticMarkup()`。

在将字符串发送给客户端进行注水之前，避免将字符串直接拼接到 `renderToStaticMarkup()` 的输出上。

为了避免 `XSS`，不要将未过滤的数据与 `renderToStaticMarkup()` 的输出连接在一起:

`app.get("/", function (req, res) {     return res.send(       ReactDOMServer.renderToStaticMarkup(         React.createElement("h1", null, "Hello ConardLi!")       ) + otherData     );   });   `

## JSON 注入

将 `JSON` 数据与服务器端渲染的 `React` 页面一起发送是很常见的。始终对 `<` 字符进行转义来避免注入攻击：

`window.JSON_DATA = ${JSON.stringify(jsonData).replace( /</g, '\\u003c')}   `

## URL 注入

前面几个基本上都是直接渲染未经过滤的富文本导致的 `XSS`，实际上通过 `URL` 伪协议也可以执行 `javascript` 脚本：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e5Dzv8p9XdSdRkhk04cqxRQrq5OOtkickFGyia8jkibricoVtMGiascecMicniaXs4p62u8iaXHbZON8u0cKiamjEIvza2w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

因此所有需要注入到代码里的 `URL` 参数，我们都要做好 `URL` 的合法性验证，千万不要直接注入进去：

`import "./styles.css";      function isSafe(url) {     if (url && !url.startsWith("http")) {       return null;     }     // ... 其他判断   }      export default function App() {     const code = "javascript:alert('xss')";     return (       <div className="App">         测试 URL 注入         <a href={isSafe(code)}>一个平平无奇的链接</a>       </div>     );   }      `

## 避免有漏洞的 React 版本

`React` 以前也被测试出有比较高危的安全漏洞，建议经常保持更新，来避免这些有漏洞的 `React` 版本：

![图片](https://mmbiz.qpic.cn/mmbiz_png/e5Dzv8p9XdSdRkhk04cqxRQrq5OOtkickhzdh9x0Nv3msJQgn0viajJA5vCumdwW9LaEIyXBPUfEiaFCFuhFuDNPQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

## 避免有漏洞的其他依赖

一般我们的项目都会依赖大量的开源代码，有时漏洞并不是我们写出来的，而是这些依赖带进来的，想详细了解可以看看我之前写的这篇文章：

[你必须要注意的依赖安全漏洞](https://mp.weixin.qq.com/s?__biz=Mzk0MDMwMzQyOA==&mid=2247490283&idx=1&sn=29075f0815976caddede220799ce744c&chksm=c2e2efc0f59566d6c4f9ce4304cf2089d86f38293d10183e1e1c70e0abf5a2bee7ea43ced272&token=1456783733&lang=zh_CN&scene=21#wechat_redirect)

因此我们无论使用任何框架，定期进行依赖更新都是不错的选择。

## Eslint React 安全配置

推荐大家通过 `Eslint` 的 `React` 安全配置（https://github.com/snyk-labs/eslint-config-react-security/）来对代码进行约束，它会自动帮助我们发现一些代码中的安全风险。