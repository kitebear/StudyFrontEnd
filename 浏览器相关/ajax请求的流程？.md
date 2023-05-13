浏览器中的 Ajax 请求流程一般包括以下几个步骤：

1. 创建 XMLHttpRequest 对象

通过 new XMLHttpRequest() 方法创建 XMLHttpRequest 请求对象，该对象提供了一些属性和方法，可以用来发送 Ajax 请求并处理响应数据。

2. 发送请求

使用 open() 方法设置请求参数和 send() 方法发送请求。open() 方法包括三个参数：请求方法（get、post 等）、请求地址和是否异步发送请求（true 或 false）。send() 方法可以发送请求数据，例如表单提交的数据。

3. 服务端响应处理

当服务端接受到请求后，会根据请求的参数进行相关的处理，并返回数据。返回的数据可以是 HTML、XML、JSON、文本等不同类型的数据。

4. 接收响应

通过 XMLHttpRequest 对象提供的 readyState 属性判断请求状态，当 readyState 值为 4 时，表示请求完成。然后通过 status 属性获取状态码，判断响应是否成功。如果状态码为 200，表示响应成功，可以通过 responseText 或 responseXML 属性获取响应数据。

5. 数据处理和展示

获取到响应数据后，可以将数据进行处理，例如解析成 HTML、JSON 或 XML 等格式，以便后续进行展示和使用。

如果请求数据失败或者请求超时，可以通过 XMLHttpRequest 对象提供的 error 和 timeout 事件进行相应处理。

需要注意的是，由于 Ajax 请求是异步进行的，因此不能直接通过返回值获取响应数据，而是需要通过回调函数的方式处理。