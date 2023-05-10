`AMD`全称为`Asynchronous Module Definition`，即异步模块定义规范。模块根据这个规范，在浏览器环境中会被异步加载，而不会像 CommonJS 规范进行同步加载，也就不会产生同步请求导致的浏览器解析过程阻塞的问题了

```js

// main.js 
define(["./print"], function (printModule) { 						    printModule.print("main"); 
});

// print.js 
define(function () { 
	return { 
		print: function (msg) { 
			console.log("print " + msg); 
		}, 
	}; 
});

```

不过 require 与 define 的区别在于前者只能加载模块，而`不能定义一个模块`。