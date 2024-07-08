本项目规范的根级目录与 Vue 脚手架的 webpack 模板的根级目录基本一致，根据自身情况，部分做了修改；

## 项目结构

整体的项目结构目录如下：

```
├── build       // 构建脚本目录， 这里面是一些webpack的配置，主要用于项目打包时的一些设置
│    └── ...
├── config      // 项目配置，这个文件夹里装的是整个项目 开发运行时的一些配置，比如npm run dev 时 项目的启动端口 之类的 
│    └── ...
├── dist        // 用于存放我们 使用 npm run build 命令打包的项目文件
│    └── ...
├── node_modules  // 用于存放我们项目的各种依赖，比如axios等等，没有moudles文件，项目就没法运行，可以使用 npm install进行项目依赖的安装,当我们拓展的安装一些别的插件时 也会装在这个文件夹里
│    └── ...
├── public      // 用于存放静态文件
│    └── ...
├── src         // (源码目录,名称不可修改) 这个文件夹 是 整个项目的主文件夹 ， 我们的代码大部分都在这里完成
│    └── ...
├── static      // 静态资源目录。不会被webpack构建，该目录会被拷贝到输出目录下；
│    └── ...
├── .babelrc                // babel 的配置文件
├── .editorconfig           // 编辑器的配置文件；可配置如缩进、空格、制表类似的参数；
├── .eslintrc.js            // eslint 的配置文件
├── .eslintignore           // eslint 的忽略规则
├── .gitignore              // git的忽略配置文件
├── .postcssrc.js           // postcss 的配置文件
├── babel.config.js         // 是一个工具链，主要用于在当前和较旧的浏览器或环境中将ECMAScript 2015+代码转换为JavaScript的向后兼容版本
├── package.json            // npm包配置文件，里面定义了项目的npm脚本，项目开发所需要模块，版本，项目名称
├── package-lock.json       // 是在 npm install时候生成一份文件，用以记录当前状态下实际安装的各个npm package的具体来源和版本号
├── README.md               // 项目的说明文档，markdown格式  
└── vue.config.js           // 保存vue配置的文件，可以用于设置代理,打包配置等
```


## build 目录

> build 目录里面是一些 webpack 的配置，主要用于项目打包时的一些设置。通常它的内部目录结构为：

```
build 
├── build.js            // 生产环境构建脚本
├── build-server.js     // 运行本地构建服务器，可以访问构建后的页面
├── dev-client.js       // 开发服务器热重载脚本，主要用来实现开发阶段的页面自动刷新
├── dev-server.js       // 运行本地开发服务器
├── check-version.js    // 检查npm、node.js版本
├── utils.js            // 构建相关工具方法
├── vue-loader.conf.js      // 配置css加载器以及编译css之后自动添加前缀
├── webpack.base.conf.js    // webpack基本配置
├── webpack.dev.conf.js     // webpack开发环境配置
└── webpack.prod.conf.js    // webpack生产环境配置
```


## config 目录

> 项目配置目录，这个文件夹里装的是整个项目 `开发、测试、运行` 时的一些配置，比如 `npm run dev` 时项目的启动端口之类的 

```
config
├── prod.env.js  // 生产环境变量
├── dev.env.js   // 开发环境变量
├── index.js     // 项目配置文件
└── test.env.js  // 测试环境变量
```


### build 和 config 目录

- build 目录下存放的是 webpack 的配置文件；
- config 目录下存放的是与项目构建相关的常用的配置选项、变量；

通常情况下，除非要配置 webpack 的 loader 或者 插件，否则，你应该优先尝试更改 config 目录下的文件


## dist 目录

> dist 目录在新建项目中一开始并不会存在。只有当你执行过一次构建命令 `npm run build` 后，才会创建。通常它的内部目录结构为：

```
dist  
├── css
├── img
├── js
├── index.html  // 项目主入口文件
└── ...  // 其他公共资源
```

这就是我们之前很熟悉的原生开发阶段的目录结构。也是浏览器能直接识别的文件类型。而我们现在使用的 Vue 等框架开发的项目，并不能为浏览器所识别，所以就需要编译打包这一步操作，来转换成实际生产环境（浏览器）所需的文件。


## public 目录

> 顾名思义，public 文件夹中存放的是项目公共资源。比如网站 LOGO 等，还会有项目的主入口文件 `index.html`。通常我们不需要对 public 文件夹内的资源做任何修改。

```
public
├──index.html    // 是一个模板文件，作用是生成项目的入口文件，webpack打包的js,css也会自动注入到该页面中。我们浏览器访问项目的时候就会默认打开生成好的index.html
└── ...
```

在 webpack 构建项目期间，webpack 插件 html-webpack-plugin 会将 /index.html 处理后并拷贝到输出目录中，并把 webpack 的构建输出资源（如：输出的 js、css 文件等等）链接自动插入到该 html 文件。此外，Vue CLI 还会自动注入资源提示（preload/prefetch）清单/图标链接（当使用PWA插件时）；


## src 目录

> (源码目录,名称不可修改) 这个文件夹是整个项目的主文件夹，我们的代码大部分都在这里完成。通常它的内部目录结构为：

```
src
├── assets  // 项目资源目录，里面主要放置一些资源文件。如：图片、图标、视频 等,这里的资源会被webpack构建
│    ├── images  // 图片
│    ├── fonts   // 字体
│    └── ...
│
├── components  // 组件文件夹，用来存放 Vue 的公共组件（注册于全局，在整个项目中通过关键词便可直接输出）
│    ├── layout  // 布局相关组件
│    │    ├── header.vue  // 头部
│    │    ├── footer.vue  // 底部
│    │    └── ...
│    │
│    ├── base  // 基础组件
│    └── ...
│
├── pages  // 页面目录, 每个页面对应的组件放入对应的目录
│    ├── home.vue  // 主页
│    │    ├── index.vue
│    │    └── ...
│    │
│    ├── user  // 用户相关页面
│    │    ├─── login.vue     // 登录页面
│    │    ├─── register.vue // 注册页面
│    │    └─── ...
│    └── ...
│
├── router // 路由,整个vue项目的路由，vue 是单页面应用的代表，这里面就是设置一个一个组件的地址的文件，可以理解为各个页面的地址路径，用于我们访问，同时可以直接在里边编写路由守卫
│    ├── index.js  // 路由主js，整合各个模块，并且还会定义一些全局钩子等其他
│    ├── modules   // 存放各个模块的路由
│    └── ...
│
├── store // 全局状态管理目录
│    ├── index.js  // 主js，整合各个模块的
│    ├── modules   // 各个模块的states
│    └── ...
│
├── common  // 全局工具方法
│    └── ...
│
├── styles // 样式文件
│    ├── reset.scss      // reset css，会在 /src/main.js 中被导入
│    ├── variables.scss  // 项目中的变量，混合（mixin）等公有样式变量
│    └── ...
│
├── App.vue  // App组件就属于根组件，其他组件的使用均需要由App组件直接或间接引入，相当于包裹整个页面的最外层的div。
├── main.js  // 项目的主js，主要作用是初始化vue实例，同时可以在此文件中引用某些组件库或者全局挂在一些变量
├── init-plugins.js  // 依赖的第三方的初始化，会在main.js中引入
└── ...
```


## static 目录

static 虽然目录可以添加任何资源，如：图片、代码 等等，但是不建议把这些资源添加到 static。 static 目录适合存放以下内容：

- 与 webpack 不兼容的库；
- 您需要在构建输出中使用具有特定名称的文件；
- 你有成千上万的图像，需要动态引用它们的路径；

static/ 目录的资源不会被 webpack 处理，它们会被拷贝到输出目录下；

要引用 static/ 目录下的资源，可以使用 publicPath/static/... ；


## 总结

这只是个通用的目录结构，可以根据实际情况进行适当调整。
