在 Vue-Router 中，提供了两种路由模式：hash 和 history。

1.  Hash 模式

Hash 模式本质上是通过锚点值（URL 中 # 号后面的部分）来实现路由的切换和监听的。当 URL 中的 hash 发生变化时，路由就会根据 hash 值的变化来切换页面，同时也可以通过监听 hashchange 事件来实现路由变化时的回调处理。举个例子，当用户访问 [http://example.com/#/home](http://example.com/#/home) 时，页面就会加载 home 组件。

优点：使用 hash 模式时，不需要服务器端做任何配置，可以直接在前端实现路由的切换和监听，同时也具有防止刷新页面时路由失效的优点。

缺点：使用 hash 模式时，URL 中会出现 # 号，可能会影响 URL 美观性，不具有传统网站的 URL 风格。

2.  History 模式

History 模式使用 HTML5 History API 来实现路由切换和监听，当用户切换路由时，需要后端服务器配合将当前路由返回给前端。当 URL 发生变化时，可以通过浏览器原生的 popstate 事件来监听路由的变化并执行相应的处理逻辑。举个例子，当用户访问 [http://example.com/home](http://example.com/home) 时，页面就会加载 home 组件。

优点：使用 history 模式时，能够实现路由更加美观，符合传统的 URL 风格，同时也能够使 URL 显示的内容更加容易被搜索引擎获取。

缺点：使用 history 模式时，需要服务器端的支持，当用户刷新页面时，需要后端服务器返回相应的页面，否则会出现 404 错误。

总之，在选择路由模式时，应根据项目具体情况选择。如果需要支持刷新页面的路由功能，可以选择 History 模式，如果只是单页面应用，不需要刷新页面，可以使用 Hash 模式。