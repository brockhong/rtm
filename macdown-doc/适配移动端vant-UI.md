##### 1.下载lib-flexible

使用的是vue-cli+webpack，通过npm来安装的



```undefined
npm i lib-flexible --save
```

##### 2.引入lib-flexible

在main.js中引入lib-flexible



```dart
import 'lib-flexible/flexible'
```

##### 3.设置meta标签

通过meta标签，设置设备宽度以及缩放比例



```xml
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
```

##### 4.安装postcss-pxtorem

一款 postcss 插件，用于将单位转化为 rem



```undefined
npm install postcss-pxtorem -D
```

##### 5.配置postcss-pxtorem

在安装postcss-pxtorem的时候会生成一个文件.postcssrc.js
 在根目录找到.postcssrc.js文件，可以在此配置的基础上根据项目需求进行修改，如：



```java
module.exports = {
  plugins: {
    //...
    'autoprefixer': {
      browsers: ['Android >= 4.0', 'iOS >= 7']
    },
    'postcss-pxtorem': {
      rootValue: 37.5, //vant-UI的官方根字体大小是37.5
      propList: ['*']
    }
  }
}
```

> 注意：在配置 postcss-loader 时，应避免 ignore node_modules 目录，这会导致 Vant 的样式无法被编译

> 温馨提示： rootValue这个配置项的数值是多少呢？？？ 通常我们是根据设计图来定这个值，原因很简单，便于开发。假如设计图给的宽度是750，我们通常就会把rootValue设置为75，这样我们写样式时，可以直接按照设计图标注的宽高来1:1还原开发。（iPhone界面尺寸：320 * 480、640 * 960、640 * 1136、750 * 1334、1080 * 1920等。）
>  那为什么你在这里写成了37.5呢？？？
>  之所以设为37.5，是为了引用像vant、mint-ui这样的第三方UI框架，因为第三方框架没有兼容rem，用的是px单位，将rootValue的值设置为设计图宽度（这里为750px）75的一半，即可以1:1还原vant、mint-ui的组件，否则会样式会有变化，例如按钮会变小。
>  既然设置成了37.5 那么我们必须在写样式时，也将值改为设计图的一半。

 <font color=#ff0000>评论说 36</font>

 

##### 6.当配置完之后，只需要重启下服务，就自动转化为rem了



```undefined
npm run dev
```

### 拓展

> px转rem不仅可以用postcss-pxtorem，同时还有px2rem-loader，只是配置不一样，详见下方链接
>  [vue：将px转化为rem，适配移动端vant-UI等框架（px2rem-loader）](https://www.jianshu.com/p/03d080c1a7e3)



