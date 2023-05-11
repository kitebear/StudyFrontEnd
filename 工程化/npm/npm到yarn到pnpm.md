### npm的问题
1. npm中node_modules是嵌套的，但是包和包之间会有相互嵌套依赖的关系，这样会导致一个项目中重复安装了很多包
![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4JbFEHzpORhVgNWMbAISibNL9rEfu2ic80rDDicLzgOrJySSGWY7SYj3dcg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)
2. windows中文件路径最长260多个字节，这样嵌套如果超过windows的路径长度会导致项目都启不起来

### yarn
1. yarn 解决了重复嵌套和路径过长的问题，它将包文件铺平，所有依赖不再是一层层嵌套的关系了，而是全部在一层
![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4JDoKzPxA8klZ5NAyzOesXHjlH8FDCibUmhwIm2PCOBafRWzuhHCBibF2g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)
展开以后大部分的包都是没有第二层node_modules的
![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4JtkJJFnCs67MKkvVicibbztSN69ibUjLkccJvJBVfGSzVMGItibwiaGyDoPw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)
当然有的包还是有node_modules嵌套的，比如说
![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4Jj1cAnRX0FfNRwVDOtaf1wYVNPkabibZDtcUGlgUleLApVPb4we9OP6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 为什么还有嵌套呢？
因为有的包可能有多个版本，提升的话只能提升一个，之后再遇到相同包不同版本的包，依然还是在使用嵌套的方式

`npm 提升到3之后，也是采用这种平铺的方式`
![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4JzMEicjaYzyuF4DY3CATA7qccznz5XqWX7TxDFFw8mKjYT8dviacVo3aQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)
当然，yarn 还实现了 yarn.lock 来锁定依赖版本的功能，不过这个 npm 也实现了。
yarn 和 npm 都采用了铺平的方案，这种方案就没有问题了么？
并不是，扁平化的方案也有相应的问题。

最主要的一个问题是幽灵依赖，也就是你明明没有声明在 dependencies 里的依赖，但在代码里却可以 require 进来。

这个也很容易理解，因为都铺平了嘛，那依赖的依赖也是可以找到的。

但是这样是有隐患的，因为没有显式依赖，万一有一天别的包不依赖这个包了，那你的代码也就不能跑了，很容易出现线上事故，因为你依赖这个包，但是现在不会被安装了。

这就是幽灵依赖的问题。

另外上面的依赖包多版本的时候只会提升一个包，那么剩下的其余版本就会重复很多次出现，并且对于空间产生浪费


### pnpm

#### Link
pnpm使用了Link，也就是软硬链接，这是操作系统提供的，硬链接相当于同一个文件目录不同的引用，而软连接就是创建了一个文件，文件的内容指向了另一个路径，相当于也是一段引用，所以这两种用起来差不多

npm通过了link的这种思想解决了，yarn和npm现阶段的问题

没错，pnpm 就是通过这种思路来实现的。

再把 node_modules 删掉，然后用 pnpm 重新装一遍，执行 pnpm install。

你会发现它打印了这样一句话：

![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4JdQnB6xBmntJqLEYs1uOiaxt2RxfEjJ70FVIYOr9SPJicosiceFtMS5mibA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

包是从全局 store 硬连接到虚拟 store 的，这里的虚拟 store 就是 node_modules/.pnpm。

我们打开 node_modules 看一下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4J2K3LeKicqS90WUnAxvMoUNqpgvWiaS4V1XnA1rnbDIcXibxMNYsZZtK6g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

确实不是扁平化的了，依赖了 express，那 node_modules 下就只有 express，没有幽灵依赖。

展开 .pnpm 看一下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4JDxzEBJEvNuAgg35OnIjPnA6oia09de0qIKV9eNa9MrEyCfNNQUxL4vA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

所有的依赖都在这里铺平了，都是从全局 store 硬连接过来的，然后包和包之间的依赖关系是通过软链接组织的。

比如 .pnpm 下的 expresss，这些都是软链接，

![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4JsgfDEBibI5feKA3T5thloA0jWJRVsx39Kbt5gEPQtIC3Bd08QQRxRUA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

也就是说，所有的依赖都是从全局 store 硬连接到了 node_modules/.pnpm 下，然后之间通过软链接来相互依赖。

官方给了一张原理图，配合着看一下就明白了：

![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4JF6r3xFh5cgURnmAG5vfG0HDKAAdoD5R1C1YhdOBN4A5w43dOwwrbGA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这就是 pnpm 的实现原理。

那么回过头来看一下，pnpm 为什么优秀呢？

首先，最大的优点是节省磁盘空间呀，一个包全局只保存一份，剩下的都是软硬连接，这得节省多少磁盘空间呀。

其次就是快，因为通过链接的方式而不是复制，自然会快。

这也是它所标榜的优点：

![图片](https://mmbiz.qpic.cn/mmbiz_png/YprkEU0TtGia6BESggSObic7byJernvP4JjZoKU9umatNVum5KpSQyvANu2R2FIGJFZfbKOmdnxtLrStharZIEMQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

相比 npm2 的优点就是不会进行同样依赖的多次复制。

相比 yarn 和 npm3+ 呢，那就是没有幽灵依赖，也不会有没有被提升的依赖依然复制多份的问题。

这就已经足够优秀了，对 yarn 和 npm 可以说是降维打击。

pnpm 是凭什么对 npm 和 yarn 降维打击的: https://mp.weixin.qq.com/s/T8mF70YUfvpVFLv1E5QwQg


## 这里没有讲pnpm对于相同包不同版本是怎么处理的？
