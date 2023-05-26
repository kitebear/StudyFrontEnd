# React18 + React-Router v6 + TypeScript + Vite2 + Ant-Design 开源管理系统

### 一、在线预览地址 👀

-   Link：[http://hooks.spicyboy.cn](http://hooks.spicyboy.cn/)

### 二、Git 仓库地址 (欢迎 Star⭐)

-   Gitee：[https://gitee.com/laramie/Hooks-Admin](https://gitee.com/laramie/Hooks-Admin)
    
-   GitHub：[https://github.com/HalseySpicy/Hooks-Admin](https://github.com/HalseySpicy/Hooks-Admin)
    

### 三、🔨🔨🔨 项目功能

-   🚀 采用最新技术找开发：React18、React-Router v6、React-Hooks、TypeScript、Vite2
-   🚀 采用 Vite2 作为项目开发、打包工具（配置了 Gzip 打包、跨域代理、打包预览工具……）
-   🚀 整个项目集成了 TypeScript （完全是为了想学习 🤣）
-   🚀 使用 redux 做状态管理，集成 immer、react-redux、redux-persist 开发
-   🚀 使用 TypeScript 对 Axios 整个二次封装 （全局错误拦截、常用请求封装、全局请求 Loading、取消重复请求……）
-   🚀 支持 Antd 组件大小切换、灰色 && 色弱模式、i18n 国际化（i18n 暂时没配置所有文件，根据项目自行配置）
-   🚀 使用 自定义高阶组件 进行路由权限拦截（403 页面）、页面按钮权限配置
-   🚀 支持 React-Router v6 路由懒加载配置、菜单手风琴模式、多标签页、面包屑导航
-   🚀 使用 Prettier 统一格式化代码，集成 Eslint、Stylelint 代码校验规范（项目规范配置）
-   🚀 使用 husky、lint-staged、commitlint、commitizen、cz-git 规范提交信息（项目规范配置）

### 四、安装使用步骤 📑

-   **Clone：**

```
# Gitee
git clone https://gitee.com/laramie/Hooks-Admin.git
# GitHub
git clone https://github.com/HalseySpicy/Hooks-Admin.git
```

-   **Install：**

```
npm install
cnpm install

# npm install 安装失败，请升级 nodejs 到 16 以上，或尝试使用以下命令：
npm install --registry=https://registry.npm.taobao.org
```

-   **Run：**

```
npm run dev
npm run serve
```

-   **Build：**

```
# 开发环境
npm run build:dev

# 测试环境
npm run build:test

# 生产环境
npm run build:pro
```

-   **Lint：**

```
# eslint 检测代码
npm run lint:eslint

# prettier 格式化代码
npm run lint:prettier

# stylelint 格式化样式
lint:stylelint
```

-   **commit：**

```
# 提交代码（会自动执行 lint:lint-staged 命令）
npm run commit
```

### 五、项目截图

#### 1、登录页：

[![hooks-login](https://camo.githubusercontent.com/baf562847bf784b2455adf6dd857ef3985a7b3e56845ea21fcb737f820c8c2e5/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f696d672f32303232303632383134323333302e706e67)](https://camo.githubusercontent.com/baf562847bf784b2455adf6dd857ef3985a7b3e56845ea21fcb737f820c8c2e5/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f696d672f32303232303632383134323333302e706e67)

#### 2、首页：

[![hooks-home](https://camo.githubusercontent.com/9dedd14d280c887356e21929e08554b94e23a5c8022d9bd029042a4379225f7a/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f696d672f32303232303632383134323334342e706e67)](https://camo.githubusercontent.com/9dedd14d280c887356e21929e08554b94e23a5c8022d9bd029042a4379225f7a/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f696d672f32303232303632383134323334342e706e67)

### 六、文件资源目录 📚

```
Geeker-Admin
├─ .vscode                # vscode推荐配置
├─ public                 # 静态资源文件（忽略打包）
├─ src
│  ├─ api                 # API 接口管理
│  ├─ assets              # 静态资源文件
│  ├─ components          # 全局组件
│  ├─ config              # 全局配置项
│  ├─ enums               # 项目枚举
│  ├─ hooks               # 常用 Hooks
│  ├─ language            # 语言国际化
│  ├─ layouts             # 框架布局
│  ├─ routers             # 路由管理
│  ├─ redux               # redux store
│  ├─ styles              # 全局样式
│  ├─ typings             # 全局 ts 声明
│  ├─ utils               # 工具库
│  ├─ views               # 项目所有页面
│  ├─ App.tsx             # 入口页面
│  ├─ main.tsx            # 入口文件
│  └─ env.d.ts            # vite 声明文件
├─ .editorconfig          # 编辑器配置（格式化）
├─ .env                   # vite 常用配置
├─ .env.development       # 开发环境配置
├─ .env.production        # 生产环境配置
├─ .env.test              # 测试环境配置
├─ .eslintignore          # 忽略 Eslint 校验
├─ .eslintrc.js           # Eslint 校验配置
├─ .gitignore             # git 提交忽略
├─ .prettierignore        # 忽略 prettier 格式化
├─ .prettierrc.js         # prettier 配置
├─ .stylelintignore       # 忽略 stylelint 格式化
├─ .stylelintrc.js        # stylelint 样式格式化配置
├─ CHANGELOG.md           # 项目更新日志
├─ commitlint.config.js   # git 提交规范配置
├─ index.html             # 入口 html
├─ LICENSE                # 开源协议文件
├─ lint-staged.config     # lint-staged 配置文件
├─ package-lock.json      # 依赖包包版本锁
├─ package.json           # 依赖包管理
├─ postcss.config.js      # postcss 配置
├─ README.md              # README 介绍
├─ tsconfig.json          # typescript 全局配置
└─ vite.config.ts         # vite 配置
```

### 七、浏览器支持

> 默认支持以下浏览器。更多浏览器可以查看 [Can I Use Es Module](https://caniuse.com/?search=ESModule)
> 
> **💢 请不要使用 QQ 浏览器开发，QQ 浏览器 不识别 某些 ES6 以上语法**

[![Edge](https://camo.githubusercontent.com/ff3b487705ee054d4deaaa0c190047b8c7413e5ee0e1d4969b2cd173d4580620/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f6d642f456467652e706e67)](https://camo.githubusercontent.com/ff3b487705ee054d4deaaa0c190047b8c7413e5ee0e1d4969b2cd173d4580620/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f6d642f456467652e706e67)

[![Firefox](https://camo.githubusercontent.com/676c235788da58b0aab21314a30c9ba3118452373f72960b63079498d3588e1b/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f6d642f46697265666f782e706e67)](https://camo.githubusercontent.com/676c235788da58b0aab21314a30c9ba3118452373f72960b63079498d3588e1b/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f6d642f46697265666f782e706e67)

[![Chrome](https://camo.githubusercontent.com/8a6e6eee85018def98cf132466d736c86d35487ced95fe19b0a4a9570094a6b9/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f6d642f4368726f6d652e706e67)](https://camo.githubusercontent.com/8a6e6eee85018def98cf132466d736c86d35487ced95fe19b0a4a9570094a6b9/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f6d642f4368726f6d652e706e67)

[![Safari](https://camo.githubusercontent.com/1026ed767f624ee9d270d93c638365bb151063990fd06ee34adce632830418c9/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f6d642f5361666172692e706e67)](https://camo.githubusercontent.com/1026ed767f624ee9d270d93c638365bb151063990fd06ee34adce632830418c9/68747470733a2f2f69616d67652d313235393239373733382e636f732e61702d6368656e6764752e6d7971636c6f75642e636f6d2f6d642f5361666172692e706e67)

last 2 versions

last 2 versions

last 2 versions

last 2 versions

### 八、项目后台接口 🧩

> 项目后台接口完全采用 Mock 数据，感谢以下 Mock 平台支持

-   FastMock： [https://www.fastmock.site/](https://www.fastmock.site/)
-   EasyMock：[https://mock.mengxuegu.com/](https://mock.mengxuegu.com/)