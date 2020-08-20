学习目标



掌握vue.js 在实战中的应用

掌握vue.js在移动H5应用

掌握工程化、组件化、模块化的开发方式

学习内容



vue.js 框架介绍

vue-cli 脚手架

vue-router 路由

vue-resource 资源

webpack 打包

es6+ eslint

工程化、组件化、模块化



Vue.js介绍

前端发展趋势

什么事MVVM 框架

什么事vue.js

对比 

vue.js 核心思想   数据驱动  组件化



vue-cli

vue-cli 脚手架介绍

vue-cli 脚手架安装

vue-cli 安装后的项目文件介绍

vue-cli 生成的项目调试

vue-c webpack Dev 配置脚本详细介绍



## Vue-cli4

Vue CLI 需要 [Node.js](https://nodejs.org/) 8.9 或更高版本 (推荐 8.11.0+)

#### 安装vue-cli

```liunx
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

```
如是3.0以下
npm install -g vue-cli@版本号
如是3.0以上
npm install -g @vue/cli@版本号
```



#### 安装是否成功

```
vue -V
@vue/cli 4.5.3
```

#### 创建项目

```
vue create test2
```

window 下使用git bash 无法使用 键盘上下箭头选择  "use arrow keys "

```
winpty vue.cmd create test2
```

<img src="vueimage\image-20200817154218521.png" alt="image-20200817154218521" style="zoom: 67%;" width="50%" height="50%" />



1. 默认选择 vue2 （babel    eslint）
2. 默认选择vue3 （babel    eslint）
3. 手动设置

按确定 手动选择

空格选择对应的插件

<img src="vueimage\image-20200817161309133.png" alt="image-20200817161309133" style="zoom:80%;" width="50%" height="50%" />



babel 按需加载

router 路由

vuex 状态机

css pre-processors 样式预过程 sass 



<img src="vueimage\image-20200817162606639.png" alt="image-20200817162606639" style="zoom:0.8;" width="50%" height="50%" />

save preset as : testPreset  在用户目录下生产 .vuerc 文件

<img src="vueimage\image-20200817163257716.png" alt="image-20200817163257716" style="zoom: 50%;" width="50%" height="50%" />

下次再次创建项目时就会出现刚预设的插件 “testPreset”。



## 编译打包配置环境变量

#### 配置打包

先看一下这篇文章   <[npm run serve/build 背后的真实操作](https://www.cnblogs.com/dengxiaoning/p/12431108.html)>

编译时打包替换不同环境的环境变量在 package.json   里的 `scripts` 配置 添加一行"stage": "vue-cli-service build --mode staging",

，通过 `--mode xxx` 来执行不同环境(替换不同环境变量)。

- 通过 `npm run serve` 启动本地 , 执行 `development`  默认寻找 .env.development
- 通过 `npm run stage` 打包测试 , 执行 `staging`   .env.staging
- 通过 `npm run build` 打包正式 , 执行 `production`  默认寻找 .env.development

```
"scripts": {
    "serve": "vue-cli-service serve",
    "stage": "vue-cli-service build --mode staging",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
```

#### 配置介绍

  以 `VUE_APP_` 开头的变量，在代码中可以通过 `process.env.VUE_APP_` 访问。
  比如,`VUE_APP_ENV = 'development'` 通过`process.env.VUE_APP_ENV` 访问。
  除了 `VUE_APP_*` 变量之外，在你的应用代码中始终可用的还有两个特殊的变量`NODE_ENV` 和`BASE_URL`

在项目根目录中新建`.env.*`

- .env.development 本地开发环境配置

```
NODE_ENV='development'
# must start with VUE_APP_
VUE_APP_ENV = 'development'
复制代码
```

<img src="vueimage\image-20200818103920272.png" alt="image-20200818103920272" style="zoom:0.8;" width="50%" height="50%" />

- .env.staging 测试环境配置

```
NODE_ENV='production'
# must start with VUE_APP_
VUE_APP_ENV = 'staging'复制代码
```

- .env.production 正式环境配置

```
 NODE_ENV='production'
# must start with VUE_APP_
VUE_APP_ENV = 'production'复制代码
```

这里我们并没有定义很多变量，只定义了基础的 VUE_APP_ENV `development` `staging` `production`
变量我们统一在 `src/config/env.*.js` 里进行管理。





**修改起来方便，不需 要重启项目，符合开发习惯**。在开发时常用

配置对应环境的变量，拿本地环境文件 `env.development.js` 举例，用户可以根据需求修改

```
// 本地环境配置
module.exports = {
  title: 'test2',
  baseUrl: 'http://localhost:8080// 项目地址
  baseApi: 'https://test.xxx.com/api', // 本地api请求地址
  APPID: 'xxx',
  APPSECRET: 'xxx'
}复制代码
```



#### config/index.js

```
// 根据环境引入不同配置 process.env.NODE_ENV
const config = require('./env.' + process.env.VUE_APP_ENV) // process.env 当前环境变量
module.exports = config复制代码
```

根据环境不同，变量就会不同了

```
// 根据环境不同引入不同baseApi地址
import {baseApi} from '@/config'
console.log(baseApi)
```



## 页面适配  px em rem

- px 固定像数
- em 相对于父元素 
- rem 相对对于根元素

默认浏览器字体大小16px;

```
html{ font-size:16px;}
```

公式:1x16=16px=1rem

假设设计稿宽度为750px,查看750px宽度的页面对应的html{font-size:XXXpx}.

假设页面宽750px，html{font-size:100px},即100px=1rem。此时想要设置一个按钮的宽度，在设计稿中按钮为200px*90px，那么转换之后的按钮即为2rem*.9rem

公式：rem = px / (font-size)

**必须设置 font-size: 以屏幕宽度计算**

```
document.documentElement.style.fontSize = document.documentElement.clientWidth 750/ *100 + 'px';
```

字体都按 /100 计算   0.32rem =32px

#### Vue 中转换rem

Vue 中使用 postcss-pxtorem  (用于单位转换)，lib-flexible 用户设置 rem 基准.

- [postcss-pxtorem](https://github.com/cuth/postcss-pxtorem) 

Install

```
$ npm install postcss-pxtorem --save-dev
```

 修改package.json

<img src="vueimage\image-20200819090619676.png" alt="image-20200819090619676" style="zoom:0.8;"  width="50%" height="50%"  />

<font color=#ff000> **"@vue/cli-service": "~6.0.0"** </font>

报错：npm audit  --json 会允许远程攻击

serialize-javascript` prior to 3.1.0 allows remote attackers to inject arbitrary code via the function 
\"deleteFunctions\" within \"index.js\

High vulnerability in dependencies -> copy webpack plugin -> serialize js

https://github.com/vuejs/vue-cli/issues/5782 



- [lib-flexible](https://github.com/amfe/lib-flexible) 

  

Install

```
npm i -S amfe-flexible
```

Import

```
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
<script src="./node_modules/amfe-flexible/index.js"></script>
```

vue 使用 lib-flexible

```
  import 'lib-flexible/flexible.js'
```

vue  Vant 中的样式使用 postcss ，根目录新建**.postcssrc.js**

```
module.exports = {
  plugins: {
    autoprefixer: {
      browsers: ['Android >= 4.0', 'iOS >= 8'],
    },
    'postcss-pxtorem': {
      rootValue: 37.5,
      propList: ['*'],
    },
  },
};
```



## VantUI 组件按需加载



#### 通过 npm 安装

```bash
# 通过 npm 安装
npm i vant -S
```

安装插件 https://github.com/ant-design/babel-plugin-import

```
npm i babel-plugin-import -D
```

配置 bable-plugin-import

```
const plugins = [
  [
    'import',
    {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    },
    'vant'
  ]
]

module.exports = {
  presets: [ '@vue/cli-plugin-babel/preset' ],
  plugins
}s
```



#### 创建vant 插件j src/plugins/vant.js

```
// 按需全局引入 vant组件
import Vue from 'vue'
import {Button, List, Cell, Tabbar, TabbarItem} from 'vant'
Vue.use(Button)
Vue.use(Cell)
Vue.use(List)
Vue.use(Tabbar).use(TabbarItem)
```

在main.js 引入

```
// 全局引入按需引入UI库 vant
import '@/plugins/vant'
```





## Sass 全局样式

#### sass的基本使用

https://www.sass.hk/docs/

- 变量

```scss
$back-ground:red;
.test{
   background:$back-ground;
}
```

- 样式嵌套 结合html嵌套更容易些样式
- 混合mixin (函数概念 可以传值、预设值)

```scss
@mixin box-border( $bcolor:#ccc){
border:1px solid $bcolor;
border-radius:4px;
}

.test{
@include box-border(red);
}
```

- 继承 @extend

```scss
extend-test{
@extend .test;
}
```

  b

}

#### 安装

```
npm install node-sass sass-loader style-loader --save
```

可能会报错重复几次，

vue-cli4安装时选择  css pre-processors 就不必这么麻烦出错的问题。

 .vue 文件之中 `scoped` 它顾名思义给 css 加了一个域的概念。

```
<style lang="scss">
    /* global styles */
</style>

<style lang="scss" scoped>
    /* local styles */
</style>
```

#### 目录结构

test2  所有全局样式都在 `@/src/assets/css` 目录下设置

```
├── assets
│   ├── css
│   │   ├── index.scss               # 全局通用样式
│   │   ├── mixin.scss               # 全局mixin   
│   │   └── variables.scss           # 全局变量复制代码
```



#### 自定义 vant-ui 样式

现在我们来说说怎么重写 `vant-ui` 样式。由于 `vant-ui` 的样式我们是在全局引入的，所以你想在某个页面里面覆盖它的样式就不能加 `scoped`，但你又想只覆盖这个页面的 `vant` 样式，你就可在它的父级加一个 `class`，用命名空间来解决问题。

```
.about-container {
    /* 你的命名空间 */
    .van-button {
        /* vant-ui 元素*/
        margin-right: 0px;
    }
}
```

#### 父组件改变子组件样式 深度选择器

当你子组件使用了 `scoped` 但在父组件又想修改子组件的样式可以 通过 `>>>` 来实现：

```
<style scoped>
.a >>> .b { /* ... */ }
</style>

编译后
.a[data-v-f3f3eg9] .b { /* ... */ }
```



或者去掉 scoped
```html
<style>
 .a .b {
  // ... }
</style>
```


https://blog.csdn.net/caseywei/article/details/99959415



#### 全局变量

`vue.config.js` 配置使用 `css.loaderOptions` 选项,注入 `sass` 的 `mixin` `variables` 到全局，不需要手动引入 ,配置`$cdn`通过变量形式引入 cdn 地址,这样向所有 Sass/Less 样式传入共享的全局变量：

```
const IS_PROD = ['production', 'prod'].includes(process.env.NODE_ENV)
const defaultSettings = require('./src/config/index.js')
module.exports = {
    css: {
        extract: IS_PROD,
        sourceMap: false,
        loaderOptions: {
            // 给 scss-loader 传递选项
            scss: {
                // 注入 `sass` 的 `mixin` `variables` 到全局, $cdn可以配置图片cdn
                // 详情: https://cli.vuejs.org/guide/css.html#passing-options-to-pre-processor-loaders
                prependData: `
                @import "assets/css/mixin.scss";
                @import "assets/css/variables.scss";
                $cdn: "${defaultSettings.$cdn}";
                 `,
            },
        },
    },
}
```

设置 js 中可以访问 `$cdn`,`.vue` 文件中使用`this.$cdn`访问

```
// 引入全局样式
import '@/assets/css/index.scss'

// 设置 js中可以访问 $cdn
// 引入cdn
import { $cdn } from '@/config'
Vue.prototype.$cdn = $cdn
```

在 css 和 js 使用

```
<script>
    console.log(this.$cdn)
</script>
<style lang="scss" scoped>
    .logo {
        width: 120px;
        height: 120px;
        background: url($cdn+'/weapp/logo.png') center / contain no-repeat;
    }
</style>
```



