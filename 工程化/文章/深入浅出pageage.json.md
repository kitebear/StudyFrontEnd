package.json就是一个json文件，除了能够描述项目的包依赖外，允许我们使用“语义化版本规则”指明你项目依赖包的版本，让你的构建更好地与其他开发者分享，便于重复使用。

-   package.json
    
-   package.json常用属性
    
-   package.json环境相关属性
    
-   package.json依赖相关属性
    
-   package.json三方属性

## 1package.json

### 1. package.json简介

在nodejs项目中，package.json是管理其依赖的配置文件，通常我们在初始化一个nodejs项目的时候会通过：

`npm init   `

然后在你的目录下会生成3个目录/文件， node_modules, package.json和 package.lock.json。其中package.json的内容为：

```
{  
    "name": "Your project name",  
    "version": "1.0.0",  
    "description": "Your project description",  
    "main": "app.js",  
    "scripts": {  
        "test": "echo \"Error: no test specified\" && exit 1",  
    },  
    "author": "Author name",  
    "license": "ISC",  
    "dependencies": {  
        "dependency1": "^1.4.0",  
        "dependency2": "^1.5.2"  
    }  
}
```

上述可以看出，package.json中包含了项目本身的元数据,以及项目的子依赖信息(比如dependicies等)。


### 2. package-lock.json

我们发现在npm init的时候，不仅生成了package.json文件，还生成了package-lock.json文件。那么为什么存在package.json的清空下，还需要生成package-lock.json文件呢。本质上package-lock.json文件是为了锁版本，在package.json中指定的子npm包比如：react: "^16.0.0"，在实际安装中，只要高于react的版本都满足package.json的要求。这样就使得根据同一个package.json文件，两次安装的子依赖版本不能保证一致。  
而package-lock文件如下所示，子依赖dependency1就详细的指定了其版本。起到lock版本的作用。
```
{  
    "name": "Your project name",  
    "version": "1.0.0",  
    "lockfileVersion": 1,  
    "requires": true,  
    "dependencies": {  
        "dependency1": {  
            "version": "1.4.0",  
            "resolved":   
"https://registry.npmjs.org/dependency1/-/dependency1-1.4.0.tgz",  
            "integrity":   
"sha512-a+UqTh4kgZg/SlGvfbzDHpgRu7AAQOmmqRHJnxhRZICKFUT91brVhNNt58CMWU9PsBbv3PDCZUHbVxuDiH2mtA=="  
        },  
        "dependency2": {  
            "version": "1.5.2",  
            "resolved":   
"https://registry.npmjs.org/dependency2/-/dependency2-1.5.2.tgz",  
            "integrity":   
"sha512-WOn21V8AhyE1QqVfPIVxe3tupJacq1xGkPTB4iagT6o+P2cAgEOOwIxMftr4+ZCTI6d551ij9j61DFr0nsP2uQ=="  
        }  
    }  
}
```

## package.json常用属性

### 2.1 script
在npm中使用script标签来定义脚本，每当制定npm run的时候，就会自动创建一个shell脚本，这里需要注意的是，npm run新建的这个 Shell，会将本地目录的node_modules/.bin子目录加入PATH变量。  
这意味着，当前目录的node_modules/.bin子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径。比如，当前项目的依赖里面有 esbuild，只要直接写esbuild xxx 就可以了。
```
{  
  // ...  
  "scripts": {  
    "build": "esbuild index.js",  
  }  
}
```

```
{  
  // ...  
  "scripts": {  
    "build": "./node_modules/.bin/esbuild index.js"   
  }  
}
```

### 2.2 bin
bin属性用来将可执行文件加载到全局环境中，指定了bin字段的npm包，一旦在全局安装，就会被加载到全局环境中，可以通过别名来执行该文件。

```
比如\@bytepack/cli的npm包：

"bin": {  
    "bytepack": "./bin/index.js"  
 },

```

一旦在全局安装了@bytepack/cli，就可以直接通过bytepack来执行相应的命令，比如

bytepack -v  
//显示1.11.0

如果非全局安装，那么会自动连接到项目的node_module/.bin目录中。与前面介绍的script标签中所说的一致，可以直接用别名来使用。

### 2.3 workspaces
在项目过大的时候，最近越来越流行monorepo。提到monorepo就绕不看workspaces，早期我们会用yarn workspaces，现在npm官方也支持了workspaces.     workspaces解决了本地文件系统中如何在一个顶层root package下管理多个子packages的问题，在workspaces声明目录下的package会软链到最上层root package的node_modules中。  
直接以官网的例子来说明：

```
{  
  "name": "my-project",  
  "workspaces": [  
    "packages/a"  
  ]  
}
```
在一个npm包名为my-project的npm包中，存在workspaces配置的目录
```
.  
+-- package.json  
+-- index.js  
`-- packages  
   +-- a  
   |  `-- package.json
```

并且该最上层的名为my-project的root包，有packages/a子包。此时，我们如果npm install,那么在root package中node_modules中安装的npm包a，指向的是本地的package/a.

```
.  
+-- node_modules  
|  `-- packages/a -> ../packages/a  
+-- package-lock.json  
+-- package.json  
`-- packages  
   +-- a  
   |   `-- package.json
```

```
-- packages/a -> ../packages/a
指的就是从node_modules中a链接到本地npm包的软链
```

## package.json环境相关属性

常见的环境，基本上分为浏览器browser和node环境两大类，接下来我们来看看package.json中，跟环境相关的配置属性。环境的定义可以简单理解如下：

-   browser环境：比如存在一些只有在浏览器中才会存在的全局变量等，比如window，Document等
    
-   node环境: npm包的源文件中存在只有在node环境中才会有的一些变量和内置包，内置函数等。

### 3.1 type
js的模块化规范包含了commonjs、CMD、UMD、AMD和ES module等，最早先在node中支持的仅仅是commonjs字段，但是从node13.2.0开始后，node正式支持了ES module规范，在package.json中可以通过type字段来声明npm包遵循的模块化规范。

```
//package.json  
{  
   name: "some package",  
   type: "module"||"commonjs"   
}
```

需要注意的是：

-   不指定type的时候，type的默认值是commonjs，不过建议npm包都指定一下type
    
-   当type字段指定值为module则采用ESModule规范
    
-   当type字段指定时，目录下的所有.js后缀结尾的文件，都遵循type所指定的模块化规范
    
-   除了type可以指定模块化规范外，通过文件的后缀来指定文件所遵循的模块化规范，以.mjs结尾的文件就是使用的ESModule规范，以.cjs结尾的遵循的是commonjs规范

### main & module & browser
除了type外，package.json中还有main,module和browser 3个字段来定义npm包的入口文件。

-   main : 定义了 npm 包的入口文件，browser 环境和 node 环境均可使用
    
-   module : 定义 npm 包的 ESM 规范的入口文件，browser 环境和 node - 环境均可使用
    
-   browser : 定义 npm 包在 browser 环境下的入口文件
    

我们来看一下这3个字段的使用场景，以及同时存在这3个字段时的优先级。我们假设有一个npm包为demo1

```
----- dist  
   |-- index.browser.js  
   |-- index.browser.mjs  
   |-- index.js  
   |-- index.mjs
```

其package.json中同时指定了main,module和browser这3个字段

```
  "main": "dist/index.js",  // main   
  "module": "dist/index.mjs", // module  
  
  // browser 可定义成和 main/module 字段一一对应的映射对象，也可以直接定义为字符串  
  "browser": {  
    "./dist/index.js": "./dist/index.browser.js", // browser+cjs  
    "./dist/index.mjs": "./dist/index.browser.mjs"  // browser+mjs  
  },  
  
  // "browser": "./dist/index.browser.js" // browser
```

默认构建和使用，比如我们在项目中引用这个npm包：
```
import demo from 'demo'
```
通过构建工具构建上述代码后，模块的加载循序为：_**browser+mjs > module > browser+cjs > main**_这个加载顺序是大部分构建工具默认的加载顺序，比如webapck、esbuild等等。可以通过相应的配置修改这个加载顺序，不过大部分场景，我们还是会遵循默认的加载顺序。


不太仔细