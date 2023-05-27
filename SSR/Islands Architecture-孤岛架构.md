## Islands Architecture是什么

`Islands Architecture`（孤岛架构）的概念最初是由**「Etsy」**的前端架构师 **「Katie Sylor-Miller」** 在 2019 年提出，并由`Preact`作者**「Jason Miller」**在islands-architecture[1]一文中推广。

这是一套基于`SSR`（服务端渲染）的架构。要了解他的特点，我们需要先了解传统`SSR`的缺陷。

在传统`SSR`中，首屏渲染时，服务端会向浏览器输出`HTML`结构。

当浏览器渲染`HTML`后，再执行前端框架的初始化逻辑，为`HTML`结构绑定事件，这一步叫`hydrate`（注水）。

当`hydrate`完成后，页面才能响应用户交互。

也就是说，只有当整个页面所有组件`hydrate`完成后，页面中任一组件才能响应用户交互。

`Chrome LightHouse`跑分中的TTI[2]（Time to Interactive，可交互时间）指标用于衡量**「页面变得完全可交互所需的时间」**。

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicMXXfFQ9IJMzqerolOkZRpFLP4H0n5Xuc8XKoNHYKjO9ssKzpbYbDUQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

传统`SSR`架构的页面随着应用体积变大，`TTI`指标会持续走高。

`孤岛架构`的目的就是为了优化`SSR`架构下`TTI`指标的问题。

在`孤岛架构`架构下，组件分为：

-   交互组件
    
-   首屏不可交互组件
    

比如在如下页面结构中：

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdiceq9xqz4ITaOI5icf39PLJfoul57wE5yOPnib4JTGFib0n6cibmCQyWL2CA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

-   **「首屏不可交互组件」**包括`Content`、`Advertisement`、`Footer`（白色部分）
    
-   **「交互组件」**包括`Header`、`Sliderbar`、`Image Carousel`（彩色部分）
    

**「首屏不可交互组件」**会像传统`SSR`一样向浏览器输出`HTML`，而**「交互组件」**会在浏览器异步、并发渲染。

**「交互组件」**就像`HTML`海洋中的孤岛，因此得名`孤岛架构`。

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicNhYqaOUTg24KzEicN9MJlhItOXl1DFlia0ytbd68O0wtMIZRN0AwI5Pg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

`孤岛架构`可以让**「交互优先级较高的组件」**优先变得可交互，剩下的低优组件再慢慢`hydrate`。

如此，在页面`hydrate`完成前，重要的组件已经可交互了，借此就能降低`TTI`指标。

`孤岛架构`的现实意义在哪呢？比如，对于一个电商网站，显然**「立刻购买按钮」**的可交互性优先级高于**「反馈按钮」**的可交互性。

`SSR`让用户能够更早看到页面，`孤岛架构`让页面中重要的部分（立刻购买按钮）可以更早被点击。这背后，就是更高的购买率，更多的钱～～～

## 实现Islands Architecture的框架

在当前，实现`孤岛架构`的全栈框架主要是`Astro`与`Qwik`。

### Astro

`Astro`的特点是：作为全栈框架，主要把控整体架构，对实现具体业务所需前端框架没有要求。

也就是说，开发者可以在`Astro`中使用`React`、`Vue`、`Preact`、`Svelte`等框架实现具体业务逻辑，甚至是在一个`.astro`组件中混用其他框架的组件。

比如，在下面例子中`.astro`组件中引入了`React`、`Vue`、`Svelte`三款框架的组件：

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicfUdaiajKtth9zEuLkibbibnM1CTOrc52PHAYyuf19A2W83Jibmof2aT7LA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### Qwik

`Qwik`的作者是`builder.io`的`CTO` **「miško hevery」**（同时也是`Angular`/`AngularJS`的发明者）。

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicRiaU5qqGs3BHYJSVn1CicAdiaX4eZrF3bguWTUoEf6M7DPQVibycKQykhg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

miško hevery

这款框架的特点是：超细粒度的`孤岛架构`，且粒度是开发者可控的。

对于`Astro`，`孤岛架构`适用的对象是组件。而在`Qwik`中，`孤岛架构`最细的粒度是**「组件中的某个方法」**。

举个例子，下面是`HelloWorld`组件（可以发现，`Qwik`采用类似`React`的语法）：

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicdYAvTibE6B84dYOvAN6FX1VHFUp77vZT0bQgw17PhyOEpcxGuBMTSJg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

对应页面渲染效果：

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicbMEJQM63RW9wyCDjW4PVgqbJmCe3Nl2MTCyTglaQUWW8nZboz4Ja2A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

打开浏览器`Network`面板，这个页面会有多少`JS`请求呢？

由于这是个静态的组件，没有逻辑，所以答案是：没有`JS`请求。

再来看看经典的计数器`Counter`组件，相比`HelloWorld`，增加了**「点击按钮状态变化的逻辑」**，代码如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicR9Fiapove9yq60uhEAUX1blqAExOKAx0pYB8XT4uk38LicQo3Lawtu3g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

对应页面渲染效果：

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdiciaGKBMcobcDOv8rauSRqQ3PCrM6Nib4sKkakycRXk4WGS6MhAGOmtIUw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

打开浏览器`Network`面板，这个页面会有多少`JS`请求呢？

答案还是：没有`JS`请求。

注意这两个组件的代码中，定义组件使用的是`component$`，有个`$`符号。

在`Counter`中，`onClick$`回调也有个`$`符号。

在`Qwik`中，后缀带`$`的函数都是**「懒加载」**的。

`孤岛架构`的粒度有多细，就取决于`$`定义的多细。

比如在`Counter`中，`onClick$`带`$`后缀，那么点击回调是懒加载的，所以首屏渲染不会包含**「点击后的逻辑」**对应的`JS`代码。

在点击按钮后，会发起2个`JS`请求，第一个请求返回的是**「点击后的逻辑」**：

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicZqicJPjhLv9j5VBz33iaQ2YI3icmrwgQnTCgcAytxAx7NtnkibRcIBVAsg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

第2个`JS`请求返回的是**「组件重新render的逻辑」**：

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicqic2BibUrDLzh3lj888Vibwajg85AKmf1IotjIyCs9oCGLeRQ6BbTSicxQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这两段代码执行后，`Counter`变为1。

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdiciaK5gm5XTxO9WfZKK42FOeu77EpEC0H8MR4rwWkWWKPoFJic450Nhqicg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

审查元素会发现，点击前，`button` `on:click`属性中保存了**「逻辑所在的地址」**：

![图片](https://mmbiz.qpic.cn/mmbiz_png/5Q3ZxrD2qNBC0icLANmIerPekkSiaYIjdicxMNiazFA9lLftibIp5wZjum8YuOVfMRMG11FrKQWx2qeK7XViczla3MVw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击后，会从对应地址下载`JS`代码，执行对应逻辑。

### React

为什么文章开头暴躁老哥吐槽`Astro`、`Qwik`没有什么新鲜理念呢，这是因为`React`很早就在朝着`孤岛架构`的理念发展了。

在`React`中，这套理念被称为Selective Hydration[3]。

具体来说，在`SSR`场景下，被`Suspense`组件包裹的组件会作为`孤岛架构`下的**「交互组件」**。

## 前端有多卷

虽然`孤岛架构`下的全栈框架有众多好处（首屏渲染快、`TTI`短），但并不是万能的。

他比较适合**「对首屏渲染速度、TTI要求高，但整体页面交互不复杂」**的场景，比如：

-   电商页面
    
-   博客
    
-   文档
    

对于**「重交互性」**的`Web`应用（比如**「后台管理系统」**、**「社区」**），更适合传统的`SSR`方案（比如`Next.js`）或`CSR`方案（直接使用前端框架）。

可见，`孤岛架构`的应用场景并不大，但他的实现难度却比`CSR`或传统`SSR`高得多。

大部分开发者，究其一生可能都不会用到`孤岛架构`。

就是这么小的细分领域，都涌现了这么多竞争对手。