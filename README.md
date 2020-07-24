# 前言

> 技术文章记录博客



联系「WeChat：13335309435」

![](_media/weixin.png)

## Python 打包之后缺少模块

```js
D:\Python>pyinstaller 3.py -F -p C:/python/lib/site-packages
```

## Python 使用pip安装一些包失败

- 一次使用
> 比如安装numpy
> 
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 库名

- 长期使用
> 修改用户文件夹下面的pip/pip.ini文件
>
内容：

```js
[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host = pypi.douban.com
```

## Python BeautifulSoup4 的使用方法

> 环境安装：

```js
pip install lxml bs4用到lxml库，如果没有安装过lxml库的时候，需要安装一下
```

> 使用方式：

```js
from bs4 import BeautifulSoup # 导入BeautifulSoup
BeautifulSoup('网络请求到的页面数据','lxml')
```

> 属性和方法：
	
```js
1.根据标签名查找
	soup.a 只能找到第一个符合要求的标签
2.获取属性
	soup.a.attrs 获取a所有的属性和属性值，返回一个字典
	soup.a.attrs[‘href’] 获取href属性
	soup.a[‘href’] 也可简写为这种形式
3.获取内容
	soup.a.string /text()
	soup.a.text //text()
	soup.a.get_text() //text()
	如果标签还是标签，那么string获取到的结果为none,而其他两个，可以获取文本内容
4.find:找到第一个符合要求的标签
	soup.find(‘a’) 找到第一个符合要求的
	soup.find(‘a’,title=‘xxx’)
	soup.find(‘a’,alt=‘xxx’)
	soup.find(‘a’,class=‘xxx’)
	soup.find(‘a’,id=‘xxx’)
5.find_All:找到所有符合要求的标签
	soup.find_All(‘a’)
	soup.find_All([‘a’,‘b’]) 找到所有的a和b标签
	soup.find_All(‘a’,limit=2) 限制前两个
6.根据选择器选择指定的内容
	select:soup.select(’#feng’)
```

> 常见的选择器：

```js
标签选择器（a）、类选择器（.）、id选择器（#）、层级选择器
层级选择器：
div .dudu #lala .name .xixi 下面好多级 div//img
div > p > a > .lala 只能是下面一级 div/img
select选择器返回永远是列表，需要通过下标提取指定对象
```

## [PxLoader](http://thinkpixellab.com/pxloader/)
> `PxLoader`是一个Javascript库，可帮助您下载图像，声音文件或您在网站上执行特定操作（例如显示用户界面或开始游戏）之前需要的其他任何内容。您可以使用它为HTML5游戏和网站创建预加载器。

## Vue本地部署

> history 模式下 页面空白不显示
> 修改config 下 `index.js` assetsPublicPath 为:'./'

```js
  mode: 'history',
  base: '/2020/7/vuetest', // 需要指定当前网站更目录
```
## 微信H5 关闭页面

```js
setTimeout(function () {
    WeixinJSBridge.call('closeWindow');
    document.addEventListener('WeixinJSBridgeReady', function () {
        WeixinJSBridge.call('closeWindow');
    }, false)
}, 1000)
```

## rem 适配屏幕尺寸

```js
fnResize()
window.onresize = function () {
  fnResize()
}
function fnResize() {
  var deviceWidth = document.documentElement.clientWidth || window.innerWidth;
  if (deviceWidth >= 750) {
	deviceWidth = 750
  }
  if (deviceWidth <= 320) {
	deviceWidth = 320
  }
  document.documentElement.style.fontSize = (deviceWidth / 3.75) + 'px'
}
```


## nuxt.js部署vue应用到服务端过程

> 配置nginx代理监听3002端口，package打包时端口3002
> 在nginx的 vhost里新建一个主机绑定

```js
upstream nodenuxt {
    server 127.0.0.1:3002; #nuxt项目 监听端口
    keepalive 64;
}

server {
    listen 80;
    server_name mysite.com;
    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;  
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Nginx-Proxy true;
        proxy_cache_bypass $http_upgrade;
        proxy_pass http://nodenuxt; #反向代理
    }
}
```

> 项目在本地完成后，npm run build 打包应用 , 打包完成后，我们将

```js
.nuxt
static
nuxt.config.js
package.json
```

>运行npm install 安装package里的依赖 
>运行npm start 就可以运行起来nuxt的服务渲染了



## 腾讯 rem.js

>用 `onorientationchange` 函数来检测屏幕旋转，在一些APP或游戏内嵌页面会有该函数不会执行、orientation获取不到的情况。所以如果是游戏app内嵌页建议使用 resize 事件，检查宽高变化来检测屏幕是否旋转。

```js
<script> 
    //屏幕适应 
    (function (win, doc) {
        if (!win.addEventListener) return;
        var html = document.documentElement;
        function setFont() {
            var html = document.documentElement;
            var k = 640;
            html.style.fontSize = html.clientWidth / k * 100 + "px";
        }
        setFont();
        setTimeout(function () {
            setFont();
        }, 300);
        doc.addEventListener('DOMContentLoaded', setFont, false);
        win.addEventListener('resize', setFont, false);
        win.addEventListener('load', setFont, false);
    })(window, document);
</script>
```



## canvas 播放视频


```js
    <div id="videoWrapper"></div>
    <script src="jsmpeg-player.min.js"></script>
    <script>
      var videoUrl = '../static/media/test_video.ts';
      new JSMpeg.VideoElement('#videoWrapper', videoUrl);
    </script> 

	https://www.npmjs.com/package/jsmpeg-player

	DEMO  :  https://cycdpo.github.io/jsmpeg-player/
```

## 微信小程序授权拒绝之后解决方法

```js
	saveImageToPhotosAlbum(option) {
        return new Promise((resolve, reject) => {
            wx.saveImageToPhotosAlbum({
                ...option,
                success: resolve,
                fail: reject,
            })
        })
    },
    save() {
        var that = this;
        this.saveImageToPhotosAlbum({
            filePath: that.data.previewImageUrl
        }).then(() => {
            wx.showToast({
                icon: 'none',
                title: '分享图片已保存至相册',
                duration: 2000
            })
        }).catch((err) => {
            console.log(err)
            let errMsg = err.errMsg
            let msg = ''
            if (errMsg === "saveImageToPhotosAlbum:fail:auth denied" || errMsg === "saveImageToPhotosAlbum:fail auth deny" || errMsg === "saveImageToPhotosAlbum:fail authorize no response") {
                // 这边微信做过调整，必须要在按钮中触发，因此需要在弹框回调中进行调用
                that.imageErrorAuth()
            } else {
                msg = '保存失败'
            }
            if (msg) {
                wx.showToast({
                    title: msg,
                    icon: 'none',
                    duration: 2000
                })
            }
        })
    },
    imageErrorAuth() {
        // 授权失败 提示授权操作
        wx.showModal({
            title: '提示',
            content: '需要您授权保存至相册',
            showCancel: false,
            success: modalSuccess => {
                wx.openSetting({
                    success(settingData) {
                        console.log("settingData", settingData)
                        if (settingData.authSetting['scope.writePhotosAlbum']) {
                            wx.showModal({
                                title: '提示',
                                content: '获取权限成功,再次保存图片即可',
                                showCancel: false
                            })
                        } else {
                            wx.showModal({
                                title: '提示',
                                content: '获取权限失败，将无法保存到相册',
                                showCancel: false
                            })
                        }
                    },
                    fail(failData) {
                        console.log("failData", failData)
                    },
                    complete(finishData) {
                        console.log("finishData", finishData)
                    }
                })
            }
        })
    },
```


## vue打包项目后刷新404的问题Nginx配置
> 这次给大家带来Vue在打包项目以后刷新显示404应该怎么处理，处理Vue在打包项目以后刷新显示404的注意事项有哪些。

```javascript
server {
  listen   80;
  server_name localhost;
  index index.html;
  root /root/dist;
  location / {
    root /root/dist;
    try_files $uri $uri/ /index.html =404;
  }
}
```


## vue使用keep-alive

>到现在，接触vue也小段时间了，项目进行到了一定程度，然而项目缺少了缓存机制，所以每次跳转页面都会重新created一下数据，虽说系统的数据请求速度很快，但是这样做对系统的性能会有很大的坏处的，所以到这里就要对系统优化下，添加缓存了。

>其实到现在，对于vue还是没有玩通，每深挖一次，就会发现一次vue的精彩，开始不清楚要用什么实现缓存，找了好久，有好几种说法，就是用vuex、vuet或者keep-alive，然后对比了一下，在我认为，vuex和vuet是实现状态管理，重心是在数据处理上；想要实现整体的缓存，阻止created的刷新，就要用keep-alive。

>所以这里我想要给大家介绍下如何用keep-alive实现缓存的页面？其实很简单，就是几句话而已。






1、keep-alive要配合router-view使用，这里要注意一点就是，`keep-alive`本身是vue2.0的功能，并不是`vue-router`的，所以再vue1.0版本是不支持的。keep-alive官方文档点这里，代码实现如下，router-view是在入口APP.vue里面
```javascript
<template>
  <div id="app">

    <keep-alive>
      <router-view></router-view>
    </keep-alive>
    
    <!--这里是其他的代码-->
  </div>
</template>
```
2、这样就会实现组件的缓存，但是有个缺点就是所有组件都会被缓存，可是现实中就是我们有些页面还是要及时刷新的，比如列表数据，想要查看详情的时候都是共用一个组件，只是刷新页面，所以这个共用的组件是不能够缓存的，不然会造成点其他的条目都是之前缓存的数据。那要怎么自定义呢，那就要在`router-view`里面多加个`v-if`判断了，然后在`router`定义的文件里面在想要缓存的页面多加上`meta:{keepAlive:true}`，不想要缓存就是`meta:{keepAlive:false}`或者不写，只有为`true`的时候是会被`keep-alive`识别然后缓存的。
```javascript
<template>
  <div id="app">
    <!--缓存想要缓存的页面，实现后退不刷新-->
    <!--加上v-if的判断，可以自定义想要缓存的组件，自定义在router里面-->
    <keep-alive>
      <router-view v-if="$route.meta.keepAlive"></router-view>
    </keep-alive>
    <router-view v-if="!$route.meta.keepAlive"></router-view>
    
    <!--这里是其他的代码-->
  </div>
</template>
```
3、在router文件加上meta判断
```javascript
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)
export default new Router({
    {//home会被缓存
        path:"/home",
        component:home,
        meta:{keepAlive: true}
    }
    {//hello不会被缓存
        path:"/hello",
        component:hello,
        meta:{keepAlive: false}
    }
})
```
想要看有没有缓存成功，可以在各个组件的created钩子里面打印输出标志，缓存成功就是首次进入页面，created会请求数据，后面就不会再次请求而是直接调用缓存的

添加了缓存可以大大减少对系统性能的损坏，毕竟做数据处理型的系统，数据过于庞大，每次都要请求一下页面是很不好的，有了缓存，该缓存的缓存，不想缓存也可以实时刷新














## 微信小程序less 编译

> 微信小程序只支持原生css写法，但是前端开发less和sass已经很普及。
> 网上有很多webstrom编译less的方法，个人觉得比较麻烦。

下面介绍一个less编译的方法：

### 全局安装wxss-cli
>npm或者yarn全局安装wxss-cli

```javascript
npm install -g wxss-cli
```

### 运行wxss-cli命令
>运行wxss-cli命令(miniProject为小程序目录)，less文件保存时自动编译

```javascript
wxss ./miniProject
```


## 微信小程序H5通讯传参

> H5 获取小程序 实时路径判断

```javascript
 window.addEventListener('popstate', function () {
     var hash = window.location.hash;
     //            document.getElementById('body').innerHTML = hash;
     //            this.hash = hash;
     //            Vue.set('hash', hash);
     that.$set(app.data, 'hash', hash);

     if (hash == '#data=2') {
         hash = '#data=1'
     }
 });
```

### 小程序端代码

```javascript
const app = getApp()

Page({
    data: {
        url: ''
    },
    onLoad: function() {
        let that = this;
        app.action._get('wce.php', function(res) {
            that.setData({
                objurl: res.data.url,
                url: res.data.url,
            });
        })
    },
    onShow: function() {
        this.setData({
            url: this.data.objurl + '#data=1'
        })
    },
    onShareAppMessage: function() {
        this.setData({
            url: this.data.objurl + '#data=2'
        })
        return {
            title: '测试',
            // imageUrl: '',
            path: '/pages/index/index'
        };
    }
})
```

## wepy采坑

wepy是一个微信小程序框架，支持模块化开发，开发风格类似Vue.js。可搭配redux使用，能同时打包出web和小程序。

官方文档 <https://tencent.github.io/wepy>

### 目录结构

`pages`: 存放主页面
`sotre`: redux(如果你创建项目时使用了`redux`的话)
`components`: 存放组件
`mixins`: 混合组件
`wepy.config.js`: `webpack`配置文件
`app.wpy`: 小程序入口文件
`project.config.json`: 小程序项目配置文件
`index.template.html`: web页面的入口文件

### mixins

mixins`是放混合组件的地方，比如很多`page`中都要用到`wx.showToast`方法。那么我们可以在`mixins`文件夹里面创建一个`toast.js

```javascript
# toast.js 
# mixins是js文件不是wpy文件
import wepy from 'wepy'

export default class testMixin extends wepy.mixin {
  onLoad () {  // onLoad生命周期钩子函数
    this.showToast()
  }
  noMore () {  // 普通方法直接定义到class的静态方法
    wepy.showToast({  // wepy.showToast 等同于 wx.showToast
      title: '没有更多了...',
      icon: 'none',
      duration: 1500
    })
  }
  showToast () {
    wepy.showToast({
      title: '拼命加载中...',
      icon: 'loading',
      duration: 3000
    })
  }
  hideToast () {
    wepy.hideToast()
  }
}
```

其中`wepy`继承了`wx`对象的方法，建议在`wepy`框架开发中不要用到`wx`对象的方法，虽然运行时效果是一样，但是打包时会`cli`报错（`wepy`中没有`wx`对象）。
`mixins`的方法定义好后，就可以在组件中使用`mixin`了。

```javascript
# index.wpy
<script>
  import wepy from 'wepy'
  import toast from 'mixins/toast' // 导入mixins组件

  export default class Index extends wepy.page {
    onLoad () {
      this.showToast() // 导入和注册后，就可直接使用
      this.getMovies()
    }
    mixins = [  // 注册混合组件，注意mixins是一个数组
      toast
    ]
    ......
  }
</script>
```

首先在引入和注册后，然后就可以直接调用`this.showToast()`
注意在`wepy`中组件中使用的是`class`,而不是`vue`中使用的`Object`。

### methods, events

在`vue`中，所有方法都定义在`methods`里面。而在wepy中，普通方法是直接定义在`class`类方法里面。`events`只定义组件间交互的方法。`methods`只定义事件方法。

```javascript
# index.wpy
getSliderImg (data) { // 普通方法
  this.sliderImg = data.slice(0, 10)
  this.$apply()
}
onPullDownRefresh () {  // 顶部下拉刷新
  this.showToast()
  this.sliderImg = null
  this.active = true
  this.$apply()
  this.getMovies()
}
events = {  // 与子组件的交互，都要写到events里面
  'showMovieDetail': (id) => {
    wepy.navigateTo({
      url: `./movie-detail?locationId=${290}&movieId=${id}`
    })
  }
}
methods = {
  toggleType (flag) { // 点击事件方法
    this.active = flag
    this.showToast()
    this.getMovies()
  }
}
```

### 关于computed

`wepy`中也有`computed`,`props`，`data`,`watch`等`vue`中有的一些属性（没有`filter`, `directive`）。
`props`,`data`,`watch`和`vue`基本无异。

`wepy`中`computed`计算属性是无法传参的（本人没能找到传参的方法，且官方文档没有提到），在处理一些动态数据的时候，只能通过其他方法来操作。
比如，服务端获取到的的JSON对象内有条时间戳数据需要转换成字符串，我的做法是将时间戳另外传值给子组件，然后在子组件中使用`computed`对`props`进行记算。

### 事件传值

`wepy`中的事件可传递一些基本类型的参数，但是需使用双括号。否则获取到的参数是字符串类型。

```javascript
 <view @tap="toggleType({{true}})">
```

引用类型的参数可以使用 微信原生的 `data-` 属性绑定数据，然后在函数中用`e.currentTarget.dataset` 获取.

```javascript
# template
<view data-movie="{{movie}}" @tap="showMovie"></view>

# script
methods = {
  showMovie (e) {
    console.log(e.currentTarget.dataset.movie) // 这样就可以获取到data属性绑定的对象
  }
}
```

### 组件传值

`wepy`组件传值的设计思路类似`vue 1.0` 。这点在官方文档讲得比较详细。需要注意是如果你需要props传递的数据跟随父组件数据变化，要使用`sync`修饰符。如果是异步获取的服务端数据，必须要在父组件使用 `this.$apply()`方法来触发子组件的刷新。
`wepy`中传递数据不能直接像`vue`中可以传递对象的属性，如

```
<child :data="data.someData"></child> // vue写法
```

但是在`wepy`中这样的写法会拿不到数据

```javascript
# father.wpy
<child :data.sync="data"></child> // 动态绑定数据需要sync修饰符

# child.wpy
# template
<text>{{someData}}</text> // 这样就能获取到someData了
# script
props = {
  data: Object
}
computed = {
  someData () {
    return this.data && this.data.someData
  }
}
```

### 动态绑定class

在`vue`中动态绑定`class`

```javascript
# vue
<div class="class-a" :class="{true ? 'class-b': 'class-c'}"></div>
```

在`wepy`中，要使用微信原生的绑定语法

```javascript
<view class="class-a {{true ? 'class-b' : 'class-c'}}">
```

其中 `class-a` 是不需要动态绑定的`class`， 双括号中才是需要绑定的`class`

### 循环渲染组件

`wepy`的循环渲染组件，必须使用 `<repeat/>`标签，或者微信官方的`<block/>`标签(这两个标签不会渲染到视图层）否则就不会渲染成功。

```javascript
# wepy 提供的repeat组件
<view class="movie" wx:if="{{movies}}">
  <repeat for="{{movies}}" key="index" index="index" item="item">
    <movie :movie.sync="item"></movie>
  </repeat>
</view>
# 微信提供的block组件
<block wx:for="{{imgArr}}" wx:key="index">
  <swiper-item class="item" data-movieId="{{item.id}}" @tap="showMovieDetail">
    <image class="img" src="{{item.img || item.image}}"></image>
  </swiper-item>
</block>
```

### wx:if

wepy中使用 `wx:if`，只阻止视图渲染，不会阻止组件初始化。
如在子组件onLoad 生命周期或者计算属性中使用了一些父级传递过来的动态数据，就会报错。


## Grace

一个精巧、易用的微信小程序开发辅助库

### 特点

1. 轻量、小巧、上手简单
2. 支持和Vue一样优雅的数据响应式
3. 支持数据自动更新、更改缓存、批量更新
4. 强大的网络功能
5. 支持全局事件总线
6. 支持跨页面传值
7. 支持mixins



### 使用

1. 将 <https://github.com/wendux/grace/blob/master/dist/grace.js> 拷贝到小程序根目录下的grace目录,并命名为index.js
2. 创建页面时用`grace.page` 替换 `Page` 即可。



```javascript
import grace from “../../grace/index.js”
grace.page({
  data: {
    userInfo: {},
    canIUse: true
  },
  onLoad() {
    //直接通过$data赋值更新数据
    this.$data.canIUse = false
    //通过$http发起网络请求
    this.$http.post(“http://www.dtworkroom.com/doris/1/2.0.0/test“, {xx: 7}).then((d) => {
      console.log(d)
    }).catch(err => {
      console.log(err.status, err.message)
    })
    //全局事件总线-监听事件
    this.$bus.$on(“enventName”, (data) => {
      console.log(data)
    })
    //返回上一页，并传递数据
    this.$goBack({retValue: “8”})
  },
  //跨页面传值
  $onBackData(data) {
    //接收页面返回的数据，
  }
  …
})
```



如果是注册组件（component）的话, 只需用 `grace.component` 替换 `Component` 构造器即可：



```javascript
// grace.component 替换 Component
grace.component({
  properties: {
  },
  data: {
     text:”我是自定义组件”,
     times:1
  },
  methods: {
    onTap(){
      //赋值更新
      this.$data.text=”自定义组件点击 +”+this.$data.times++
    }
  }
}
```

**注意：Grace 注入到实例中的所有方法和属性名都以“$”开始。**

### 数据响应式

微信小程序中数据发生变化后都要通过setData显式更新如：

```javascript
//更新单个字段
this.setData({
    userInfo: res.userInfo
 })
//更新多个字段
this.setData({
    userInfo: res.userInfo
    canIUse: false
})
```

这很明显是受了React的影响，好的不学，如果你用过Vue, 你应该会觉得这看起来很不优雅，尤其是代码中零零散散要更新的值多的时候，代码看起来会很冗余，还有，有时为了改变一个变量，也得调一次`setData`.

现在，有了Grace， 它会让你的代码变的优雅，你可以像使用Vue一样更新数据：

```javascript
this.$data.userInfo=res.userInfo;
//更新多个字段，并非重新赋值
this.$data={
    userInfo: res.userInfo
    canIUse: false
}
```

现在，你可以直接通过赋值就能更新界面了。当然，您依旧可以使用`this.setData`来更新数据，grace会自动同步 `this.$data`.

#### 数组更新检测

grace的数据响应式原理和Vue是一样的，（如果你熟悉Vue，可以跳过）对于数组：

#### 变异方法

grace包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

#### 替换数组

变异方法 (mutation method)，顾名思义，会改变被这些方法调用的原始数组。相比之下，也有非变异 (non-mutating method) 方法，例如：`filter()`, `concat()` 和 `slice()` 。这些不会改变原始数组，但**总是返回一个新数组**。当使用非变异方法时，可以用新数组替换旧数组：



```javascript
this.$data.items = this.$data.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```



#### 注意事项

由于 JavaScript 的限制，grace不能检测以下变动的数组：

1. 当你利用索引直接设置一个项时，例如：`this.$data.items[indexOfItem] = newValue`
2. 当你修改数组的长度时，例如：`this.$data.items.length = newLength`

为了解决第一类问题，以下两种方式都可以实现和 `this.$data.items[indexOfItem] = newValue` 相同的效果，同时也将触发状态更新：

```javascript
this.$data.$set(example1.items, indexOfItem, newValue)
// Array.prototype.splice
this.$data.items.splice(indexOfItem, 1, newValue)
```

为了解决第二类问题，你可以使用 `splice`：

```javascript
this.$data.items.splice(newLength)
```

#### 对象属性的添加

还是由于 JavaScript 的限制，**grace 不能检测对象属性的添加或删除**：

```javascript
grace.page({
  data: {
    a: 1
  }
  onLoad(){
   //a现在是响应式的
   this.$data.a=2;
   //b不是响应式的
   this.$data.b = 2
  }
})
```

如果需要动态添加响应式属性，可以使用 `$data.$set(object, key, value)` ，例如：

```javascript
this.$data.$set(this.$data, ‘b’, 2)
```

#### 数据变更缓存

根据微信[小程序官方优化建议](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/performance/tips.html)，grace可以避免如下问题：

1. **频繁的去 setData**

   为了解决这个问题，grace引入了数据变更缓存机制，下面看一个例子：

   ```javascript
   //开始缓存数据变更
   this.$data.$cache();
   
   //接下来是n次密集的数据更新
   this.$data.name=”doris”
   this.$data.userCard.no=”610xxx889”
   this.$data.balance=66666
   ….
   //统一提交变更
   this.$data.$commit();
   ```

   在调用`$cache()`之后，所有数据的变化将会缓存起来（不会触发`setData`）, 直到调用 `$commit`后，才会统一刷新，这样可以避免了频繁调用`setData`带来的性能消耗。

2. **后台态页面进行 setData**

   当页面进入后台态（用户不可见），不应该继续去进行`setData`，后台态页面的渲染用户是无法感受的，另外后台态页面去`setData`也会抢占前台页面的执行。当页面进入后台时，grace会自动停止数据更新，当页面再次转到前台时会自动开启渲染，而无需您手动去切换。

### Http

Grace通过Promise封装了wx.request， 并支持拦截器、请求配置等：

1. Restful API

   ```javascript
   $http.get(url, [data], [options])
   $http.post(url, data, [options])
   $http.put(url, data, [options])
   $http.delete(url,[data],[options])
   $http.patch(url,[data],[options])
   ```

2. 多个并发请求

   ```javascript
   var getUserRecords=()=>{
     return this.$http.get(‘/user/133/records’);
   }
   
   var getUserProjects=()=>{
     return this.$http.get(‘/user/133/projects’);
   }
   
   this.$http.all([getUserRecords(), getUserProjects()])
     .then(this.$http.spread(function (records, projects) {
       // Both requests are now complete
     }))
     .catch(function(error){
       console.log(error)
     })
   ```

3. 拦截器

   ```javascript
   // Add a request interceptor
   this.$http.interceptors.request.use((request)=>{
       // Do something before request is sent
        request.headers[“X-Tag”]=”grace”;
       // Complete the request with custom data
       // return Promise.resolve(“fake data”)
   })
   
   // Add a response interceptor
   this.$http.interceptors.response.use(
       (response) => {
           // Do something with response data .
           // Just return the data field of response
           return response.data
       },
       (err) => {
         // Do something with response error
         // return Promise.resolve(“ssss”)
       }
   )
   ```

Grace使用的http请求库是 [FLY](https://github.com/wendux/fly) , `$http`是 [FLY](https://github.com/wendux/fly)的一个实例，详情可以参照其官网，如果您想创建新的 [FLY](https://github.com/wendux/fly) 实例：

```javascript
var newHttp=grace.creatHttpClient();
```

**注意：grace创建页面时，所有页面的$http都是同一个FLY 实例，你可以使用 grace.http 获取该实例 ，所以对 grace.http的配置，会在全局生效。**

所以如果你想要配置全局的拦截器、请求基地址、超时时间等可以创建一个帮助文件，然后页面引入这个文件即可：

```javascript
import grace from “../grace/index.js”
grace.http.config.baseURL = ‘http://www.dtworkroom.com/doris/1/2.0.0/‘
grace.http.config.timeout = 5000;
grace.http.interceptors.request.use((config, promise) => {
    //拦截器逻辑
    //console.log(config.body);
});
export default grace;
```

### 事件总线

全局事件总线可以在全局（跨页面）触发、监听事件。

**$on(eventName,handler)**

监听事件

```javascript
this.$bus.$on(“enventName”,(arg1,arg2)=>{
      //事件处理器参数为$emit触发事件时传递的参数
        console.log(arg1)
})
```

**$emit(eventName,[…arguments])**

触发事件

```javascript
this.$bus.$emit(“enventName”, 1,2)
```

**$off(eventName,[handler])**

取消监听

```javascript
this.$bus.$off(“eventName”,handler)
```

当提供hanlder时，只将该hanlder移出监听者队列，如果没有传handler,则清空该事件的监听者队列。

**注意，全局只有一个$bus实例，您也可以使用 grace.bus 替代 this.$bus.**

### 跨页面传值

在小程序中打开新页面时可以通过url的query向新页面传值，这很容易，如：

```javascript
wx.navigateTo({
  //传递id,在新页面onLoad中获取
  url: ‘test?id=1’
})
```

但是，新页面关闭时如何向前一个页面返回数据？ 小程序中没有提供直接的方法，grace给所有页面添加了一个回调，用于接收页面回传的数据，如下：

```javascript
grace.page({
  data:{}
  $onBackData(data){
   //接收页面返回的数据，
  }
  …
})
```

上面的页面我们记为A, 假设你打开了一个新页面B, 你需要在B中选择一些信息后回传给A，那么你在B中应该：

```javascript
grace.page({
  data: {},
  bindViewTap(){
    //返回上一个页面，并回传一些数据
    this.$goBack({xxx:5});
  }
  …
}
```

**$goBack([data],[delta])**

关闭当前页面，返回上一页面或多级页面，如果存在`data`, 则会调用返回到的页面的`$onBackData`回调，若`data`不存在，则不会回调`$onBackData`.

`delta` 意义同 `wx.navigateBack`参数的delta, 表示回退的页面数，默认为1（上一页），如果如果 delta 大于现有页面数，则返回到首页。

### mixin

混入 (mixins) 是一种分发页面（Page）可复用功能的非常灵活的方式。简而言之，他可以在小程序创建页面时，混合页面选项，可以实现给所有页面添加一些钩子的功能，如果还不理解，不要紧，下面来看一个例子：

实现：在任何页面调用`onLoad`、`onShow` 时打印日志，并输出当前页面id.

1. 创建一个help.js文件

   ```javascript
   import grace from “../grace/index.js”
   var page=grace.page;
   grace.page=function(ob){
     mixin(ob,{
       onLoad(){
        //页面调用onShow时打印出当前页面id
        console.log(“onLoad, pageID:”+this.$id)
       },
       onShow(){
        //页面调用onShow时打印出当前页面id
        console.log(“onShow, pageID:”+this.$id)
         }
     })
     //创建页面
     page.call(grace,ob)
   }
   export default grace;
   ```

2. 在创建Page时引入help.js

   ```javascript
   import grace from “../../utils/help.js”
   grace.page({
     data:{}
   })
   
   //控制台输出
   > onLoad, pageID:1
   > onShow, pageID:1
   ```

这样一来，相当于给所有的Page添加了`onLoad`、`onShow` 预处理。

可以看到，mixin通过混入页面创建参数给页面添加统一的预处理功能，相当于添加了页面钩子。

### 选项合并

当页面构建对象和混入对象含有同名选项时，这些选项将以恰当的方式混合。

1. 数据对象在内部会进行浅合并 (一层属性深度)，在和页面构建数对象发生冲突时以页面构建数对象数据优先。

2. 同名钩子函数将混合为一个数组，因此都将被调用。另外，混入对象的钩子将在页面自身钩子**之前**调用。

   ```javascript
   grace.mixin(ob,{
       onShow(){
        console.log(“mixin onShow”)
       }
    });
   
   …
   
   grace.page({
      onShow(){
        console.log(“page onShow”)
      }
   })
   
   //页面显示时会输出：
   > mixin onShow
   > page onShow
   ```
```
   
## 微信模版消息

### 拼装模板，创建通知内容
​```js
    private function create_template($form_id) {
        $templateData['keyword1']['value'] = '进口金枕头榴莲（S号）1个54.9元【香甜软糯】';
        $templateData['keyword2']['value'] = '2104-12-09 16:00';
        $templateData['keyword3']['value'] = 'Mr.xu';
        $templateData['keyword4']['value'] = '13335309435';
        $templateData['keyword5']['value'] = '11';
        $templateData['keyword6']['value'] = '陕西省西安市未央区阿方二路小米小区2号楼4单元4楼东户';
        $data['touser'] = 'oIO0n4xTAvSIJ-H4jeA9PdUJIu1o';
        $data['template_id'] = 'iBlngRhAJIn9yTQAYUgnJU5eM3prTYq3duy_gVligKQ';
        $data['page'] = 'pages/index/index'; //用户点击模板消息后的跳转页面
        $data['form_id'] = $form_id;
        $data['data'] = $templateData;
        return json_encode($data);
    }
```

### 执行模板消息发布

```js
    public function send_notice() {
        $form_id = I('form_id', '', 'strip_tags');
        $access_token = "access_token ";
        $content = '通知内容'; //可通过$key作为键来获取对应的通知数据
        if ($access_token) {
            $templateData = $this->create_template($form_id); //拼接模板数据

            $res = json_decode($this->post('https://api.weixin.qq.com/cgi-bin/message/wxopen/template/send?access_token=' . $access_token, $templateData, true));

            return $res;
        }
    }
```


## 小程序canvas api生成分享海报图 解决方案


  ![img](http://blog.wxall.top/uploads/180802/2-1PP21IJ4562.png)

实际上使用了小程序提供的绘图api，文档在此 [小程序画布](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/reference.html)[canvas ](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/reference.html)[API](https://developers.weixin.qq.com/miniprogram/dev/api/canvas/reference.html)，其他还有getImageInfo和saveImageToPhotosAlbum等。

在wxml准备好一个画布，给定一个画布id，宽高见js部分。



```js
<!-- 生成图片的画布 -->
  <canvas class='firstCanvas' 
          style="width:{{windowWidth}}px;height:{{windowHeight}}px;box-shadow: 1px 1px 5px 5px rgba(0, 0, 0, 0.05);" 
          canvas-id="firstCanvas">
</canvas>
<!-- 按钮 -->
<button bindtap='buildPosterSaveAlbum'>生成海报</button>
```

```javascript
data: {
    // 屏幕可用宽高
    windowWidth: wx.getSystemInfoSync().windowWidth,
    windowHeight: wx.getSystemInfoSync().screenHeight,
    // 图片预览本地文件路径
    previewImageUrl: null
  },
```

```js
buildPosterSaveAlbum: function() {
    let that = this;
    wx.showLoading({
      title: '图片生成中',
    })
 
    // 获取图1信息
    // tip 貌似本地静态文件路径不能作为画布的src参数，网络图片无影响。
    let promise1 = new Promise(function(resolve, reject) {
      wx.getImageInfo({
        src: 'xxx1.png',
        success: function(res) {
          resolve(res);
        },
        fail: function(res) {
          reject(res)
        }
      })
    });
 
    // 获取图2信息
    let promise2 = new Promise(function(resolve, reject) {
      wx.getImageInfo({
        src: 'xxx2.png',
        success: function(res) {
          resolve(res);
        },
        fail: function(res) {
          reject(res)
        }
      })
    });
 
    // 执行
    Promise.all(
      [promise1, promise2]
    ).then(res => {
 
      // 获取宽高
      let wW = that.data.windowWidth;
      let wH = that.data.windowHeight;
 
      // 定义画布上下文常量
      const ctx = wx.createCanvasContext('firstCanvas');
 
      //背景白色
      ctx.setFillStyle('white');
      //从x=0,y=0开始绘制白色
      ctx.fillRect(0, 0, wW, wH);
 
      /* 绘制图像到画布  图片的位置你自己计算好就行 参数的含义看文档 */
      //图1
      ctx.drawImage(res[0].path, wW * 0.05, wW * 0.2, wW * 0.9, wW * 0.9);
      //图2
      ctx.drawImage(res[1].path, wW * 0.05, wW * 0.05, wW * 0.1, wW * 0.1);
      /* 绘制文字 位置自己计算 参数自己看文档 */
      ctx.setFillStyle('#000000');
      ctx.setFontSize(15);
 
      let str = '目标文字';
      // 绘制文字
      ctx.fillText(str, wW * 0.05, wW * 1.17)
 
      /*保存上下文 绘制 */
      // ctx.save();
      ctx.draw();
 
      //destWidth值越大图片越清晰/大小成正比 解决画布模糊的问题
      //详细的参数见画布文档
      wx.canvasToTempFilePath({
        canvasId: 'firstCanvas',
        width: wW,
        height: wH,
        destWidth: wW * 3,
        destHeight: wH * 3,
        quality: 1,
        success: function success(res) {
          console.log('转图片结果');
          // 关闭loading
          wx.hideLoading();
          // 到page对象的data中
          that.setData({
            previewImageUrl: res.tempFilePath
          })
 
          //可写成函数调用 这里不做解释
          wx.saveImageToPhotosAlbum({
            filePath: that.data.previewImageUrl,
            success(res) {
              //保存成功
              console.log(res);
            },
            fail: function(res) {
              wx.showToast({
                icon: 'fail',
                title: 'sorry 保存失败,请稍后再试',
              })
              return;
            }
          })
        },
        complete: function complete(e) {
          console.log(e.errMsg);
        }
      });
    }).
    catch(err => {
      //error 错误处理
    })
  }
```

## 微信小程序封装

```js
/*
 * @Author: 开黑店养只猫
 * @Date: 2019-05-29 10:15:03 
 * @Desc: 小程序配置文件
 * @name: 医院小程序
 * @Last Modified time: 2019-05-29 10:15:03 
 */
const wechatName = '医院小程序';
const host = 'host';

var config = {
    // 小程序请求接口
    host,
    // 微信授权登录
    login: `${host}login`,
    // 获取openid
    getOpenid: `${host}getOpenid`,
    // 进入首页加载
    init: `${host}init`,
};


/**
 * 小程序主动更新
 */
function updateManager() {
    // 获取小程序更新机制兼容
    if (wx.canIUse('getUpdateManager')) {
        const updateManager = wx.getUpdateManager()
        updateManager.onCheckForUpdate(function(res) {
            // 请求完新版本信息的回调
            if (res.hasUpdate) {
                updateManager.onUpdateReady(function() {
                    wx.showModal({
                        title: '更新提示',
                        content: '新版本已经准备好，是否重启应用？',
                        showCancel: false,
                        success: function(res) {
                            if (res.confirm) {
                                // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
                                updateManager.applyUpdate()
                            }
                        }
                    })
                })
                updateManager.onUpdateFailed(function() {
                    // 新的版本下载失败
                    wx.showModal({
                        title: '已经有新版本了哟~',
                        showCancel: false,
                        content: '请在微信 “发现-小程序” 页删除当前小程序，重新通过小程序名称搜索打开使用最新版本',
                    })
                })
            }
        })
    } else {
        // 如果希望用户在最新版本的客户端上体验您的小程序，可以这样子提示
        wx.showModal({
            title: '提示',
            showCancel: false,
            content: '当前微信版本过低,请升级到最新微信版本。'
        })
    }
}


function log(obj) {
    if (typeof obj === 'string') {
        console.log("%c " + obj, "color:#0a0;font-size:3em;");
    } else {
        console.log(obj);
    }
}


function __args() {
    var setting = {};
    if (arguments.length === 1 && typeof arguments[0] !== 'string') {
        setting = arguments[0];
    } else {
        setting.url = arguments[0];
        if (typeof arguments[1] === 'object') {
            setting.data = arguments[1];
            setting.success = arguments[2];
        } else {
            setting.success = arguments[1];
        }
    }
    if (setting.url.indexOf('http://') !== 0) {

        setting.url = setting.url;
    }
    return setting;
}

function __json(method, setting) {
    setting.method = method;
    setting.header = {
        'content-type': 'application/x-www-form-urlencoded'
    };
    wx.request(setting);
}


function _get() { // get请求
    __json('GET', __args.apply(this, arguments));
}

function _post() { //post请求
    __json('POST', __args.apply(this, arguments));
}

function to_date(phpstr, type) { // php时间戳转日期
    var str = parseInt(phpstr) * 1000;
    var newDate = new Date(str);
    var year = newDate.getUTCFullYear(); //取年份
    var month = (newDate.getUTCMonth() + 1) >= 10 ? (newDate.getUTCMonth() + 1) : '0' + (newDate.getUTCMonth() + 1); //取月份
    var nowday = newDate.getUTCDate() >= 10 ? newDate.getUTCDate() : '0' + newDate.getUTCDate(); //取天数
    var hours = newDate.getHours() >= 10 ? newDate.getHours() : '0' + newDate.getHours(); //取小时
    var minutes = newDate.getMinutes() >= 10 ? newDate.getMinutes() : '0' + newDate.getMinutes(); //取分钟
    var seconds = newDate.getSeconds() >= 10 ? newDate.getSeconds() : '0' + newDate.getSeconds(); //取秒
    var newdatastr = '';
    if (type == 1) { //年月日
        newdatastr = year + "-" + month + "-" + nowday;
    }
    if (type == 2) { //时分秒
        newdatastr = hours + ":" + minutes + ":" + seconds;
    }
    if (type == 3) { //年月日时分秒
        newdatastr = year + "-" + month + "-" + nowday + " " + hours + ":" + minutes + ":" + seconds;
    }
    return newdatastr;
}

function removeAllSpace(str) { //删除空格
    return str.replace(/\s+/g, "");
}

function random(Max, Min) { // 随机数
    var Range = Max - Min;
    var Rand = Math.random();
    return (Min + Math.round(Rand * Range));
}


function showSuccess(msg, callback) { //显示成功提示框
    wx.showToast({
        title: msg,
        icon: 'success',
        mask: true,
        duration: 1500,
        success: function() {
            callback && (setTimeout(function() {
                callback();
            }, 1500));
        }
    });
}

function showError(msg, callback) { //显示 showModal 提示框
    wx.showModal({
        title: '微信提示',
        content: msg,
        showCancel: false,
        success: function(res) {
            callback && callback();
        }
    });
}


function urlEncode(data) { //对象转URL
    var _result = [];
    for (var key in data) {
        var value = data[key];
        if (value.constructor == Array) {
            value.forEach(function(_value) {
                _result.push(key + "=" + _value);
            });
        } else {
            _result.push(key + '=' + value);
        }
    }
    return _result.join('&');
}

function alert(title) { // 提示
    wx.showToast({
        title: title,
        icon: 'none'
    })
}

function sequenceTasks(tasks) { //图片上传-顺序处理函数  
    //记录返回值
    function recordValue(results, value) {
        results.push(value);
        return results;
    }
    let pushValue = recordValue.bind(null, []);
    let promise = Promise.resolve();
    // 处理tasks数组中的每个函数对象
    for (let i = 0; i < tasks.length; i++) {
        let task = tasks[i];
        promise = promise.then(task).then(pushValue);
    }
    return promise;
}

function uploadFile(num, success) { //上传照片 上传数量  回调函数
    var imgindex = 0;
    var imgarr = []; //上传保存图片路径
    wx.chooseImage({
        count: num ? num : 1,
        sizeType: ['original', 'compressed'],
        sourceType: ['album', 'camera'],
        success(res) {
            const tempFilePaths = res.tempFilePaths;
            // console.log(tempFilePaths);
            // return false;
            wx.showLoading({
                title: "上传中"
            });
            //函数数组，每个函数的返回值是一个promise对象
            let promiseFuncArr = [];
            //图片地址数组
            let imageList = [];
            //将图片地址的上传的函数加入到promiseFuncArr数组中
            for (let i = 0; i < tempFilePaths.length; i++) {
                let promiseTemp = function() {
                    return new Promise((resolve, reject) => {
                        //微信图片上传
                        wx.uploadFile({
                            url: `${config.uploadFile}`,
                            filePath: tempFilePaths[i],
                            name: 'upfile',
                            success: function(res) {
                                //可以对res进行处理，然后resolve返回
                                resolve(res);
                            },
                            fail: function(error) {
                                reject(error);
                            },
                            complete: function(res) {},
                        })
                    });
                };
                promiseFuncArr.push(promiseTemp)
            }

            sequenceTasks(promiseFuncArr).then((result) => {
                console.log(result);
                result.map(v => {
                    imgarr.push(JSON.parse(v.data).url);
                });
                wx.hideLoading();
                success && success(imgarr);
                //对返回的result数组进行处理
            });
        }
    })
}

function getsys(key) { //获取本地存储
    return wx.getStorageSync(key);
}

function setsys(key, value) { //设置本地存储
    wx.setStorageSync(key, value);
}

function delsys(key) { //删除本地存储
    wx.removeStorageSync(key);
}

function gourl(url) { //跳转到指定页面
    if (!url || url.length == 0) {
        return false;
    }
    wx.navigateTo({
        url: url
    });
}


function gotab(url) { //跳转tabBar页面
    if (!url || url.length == 0) {
        return false;
    }
    wx.switchTab({
        url: url
    });
}
/**
 *  把html转义成HTML实体字符
 */
function htmlEncode(str) {
    var s = "";
    if (str.length === 0) {
        return "";
    }
    s = str.replace(/&/g, "&amp;");
    s = s.replace(/</g, "&lt;");
    s = s.replace(/>/g, "&gt;");
    s = s.replace(/ /g, "&nbsp;");
    s = s.replace(/\'/g, "&#39;"); //IE下不支持实体名称
    s = s.replace(/\"/g, "&quot;");
    return s;
}


/**
 *  转义字符还原成html字符
 */
function htmlRestore(str) {
    var s = "";
    if (str.length === 0) {
        return "";
    }
    s = str.replace(/&amp;/g, "&");
    s = s.replace(/&lt;/g, "<");
    s = s.replace(/&gt;/g, ">");
    s = s.replace(/&nbsp;/g, " ");
    s = s.replace(/&#39;/g, "\'");
    s = s.replace(/&quot;/g, "\"");
    return s;
}
/**
 * 去掉所有的html标签和&nbsp;之类的特殊符合
 */
function deleteHtmlTag(str) {
    str = str.replace(/<[^>]+>|&[^>]+;/g, "").trim();
    return str;
}

// 微信小程序rich-text富文本图片自适应处理
function richtext(data) {
    var result = data;
    result = result.replace(/\<img/gi, '<img style="max-width:100%;height:auto" ');
    result = result.replace(/section/gi, 'div');
    return result;
}
// 获取 自定义  data 属性
function tapinfo(e) {
    return e.currentTarget.dataset;
}
/**
 * 获取php 时间戳 几小时之前
 */
function getDateDiff(dateStr) {
    var publishTime = parseInt(dateStr) * 1000,
        d_seconds,
        d_minutes,
        d_hours,
        d_days,
        timeNow = parseInt(new Date().getTime()),
        d,

        date = new Date(publishTime),
        Y = date.getFullYear(),
        M = date.getMonth() + 1,
        D = date.getDate(),
        H = date.getHours(),
        m = date.getMinutes(),
        s = date.getSeconds();



    //小于10的在前面补0
    if (M < 10) {
        M = '0' + M;
    }
    if (D < 10) {
        D = '0' + D;
    }
    if (H < 10) {
        H = '0' + H;
    }
    if (m < 10) {
        m = '0' + m;
    }
    if (s < 10) {
        s = '0' + s;
    }
    d = timeNow - publishTime;
    var miao = 1000;
    var minute = miao * 60;
    var hour = minute * 60;
    var day = hour * 24;
    var halfamonth = day * 15;
    var month = day * 30;

    d_days = parseInt(d / day);
    d_hours = parseInt(d / hour);
    d_minutes = parseInt(d / minute);
    d_seconds = parseInt(d / miao);

    if (d_days > 0 && d_days < 7) {
        return d_days + '天前';
    } else if (d_days <= 0 && d_hours > 0) {
        return d_hours + '小时前';
    } else if (d_hours <= 0 && d_minutes > 0) {
        return d_minutes + '分钟前';
    } else if (d_seconds < 60) {
        if (d_seconds <= 0) {
            return '刚刚发表';
        } else {
            return d_seconds + '秒前';
        }
    } else if (d_days >= 7 && d_days < 30) {
        return M + '-' + D + ' ' + H + ':' + m;
    } else if (d_days >= 30) {
        return Y + '-' + M + '-' + D + '' + H + ':' + m;
    }
}

function islogin() {
    //	检测用户是否授权
    wx.getSetting({
        success(res) {
            if (!res.authSetting['scope.userInfo']) {
                wx.navigateTo({
                    url: '/pages/login/login'
                });
            }
        }
    })
},

function getOpenid(cb) { //	获取openid
	let app = this;
	wx.login({
		success(res) {
			if (res.code) {
				app._post(app.config.getOpenid, {
					code: res.code
				}, function(res) {
					if (res.data.code == 0) {
						wx.setStorageSync('openid', res.data.data.openid);
						// wx.setStorageSync('userid', res.data.data.userid)
						cb && cb();
					}
				});
			} else {
				app.getOpenid();
				console.log('登录失败！' + res.errMsg)
			}
		}
	});
}


function Rad(d) { //根据经纬度判断距离
	return d * Math.PI / 180.0;
},
function getDistance(lat1, lng1, lat2, lng2) {
	// lat1用户的纬度
	// lng1用户的经度
	// lat2商家的纬度
	// lng2商家的经度
	var radLat1 = Rad(lat1);
	var radLat2 = Rad(lat2);
	var a = radLat1 - radLat2;
	var b = Rad(lng1) - Rad(lng2);
	var s = 2 * Math.asin(Math.sqrt(Math.pow(Math.sin(a / 2), 2) + Math.cos(radLat1) * Math.cos(radLat2) * Math.pow(Math.sin(b / 2), 2)));
	s = s * 6378.137;
	s = Math.round(s * 10000) / 10000;
	s = s.toFixed(1); //保留两位小数
	console.log('经纬度计算的距离:' + s)
	return s
},
module.exports = {
    config: config,
    to_date: to_date,
    removeAllSpace: removeAllSpace,
    random: random,
    urlEncode: urlEncode,
    showSuccess: showSuccess,
    showError: showError,
    alert: alert,
    uploadFile: uploadFile,
    getsys: getsys,
    setsys: setsys,
    delsys: delsys,
    gourl: gourl,
    _get: _get,
    _post: _post,
    gotab: gotab,
    htmlRestore: htmlRestore,
    htmlEncode: htmlEncode,
    log: log,
    tapinfo: tapinfo,
    richtext: richtext,
    getDateDiff: getDateDiff,
    deleteHtmlTag: deleteHtmlTag,
    updateManager: updateManager,
    getDistance:getDistance,
    islogin:islogin,
    getOpenid:getOpenid
}
```

## 从网页向小程序发送消息

查阅[小程序官方文档](https://developers.weixin.qq.com/miniprogram/dev/component/web-view.html?search-key=wx.miniProgram.postMessage)，有这样一个接口 `wx.miniProgram.postMessage` ，可以用来从网页向小程序发送消息，然后通过 `bindmessage` 事件来监听消息，如下是官方文档描述

![clipboard.png](../_media/403726539-5bc708dee6924_articlex.png)

以下是代码：

```html
// 网页代码
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
        <title>postMessage</title>
    </head>

    <body>
        <script type="text/javascript" src="https://res.wx.qq.com/open/js/jweixin-1.3.2.js"></script>
        <script type="text/javascript">
            wx.miniProgram.postMessage({ data: '获取成功' })
            
            wx.miniProgram.navigateBack({delta: 1})
        </script>
    </body>
</html>
// 小程序代码
<web-view bindmessage="handleGetMessage" src="test.html"></web-view>

Page({
  handleGetMessage: function(e) {
    console.log(e.target.data)
  }
})
```

**写完试了下，期待打印 “获取成功” ，而实际小程序里面啥也没打印。。。**

然后仔细看官方文档，发现有这句话：

> 网页向小程序 `postMessage` 时，会在**特定时机（小程序后退、组件销毁、分享）触发**并收到消息。

也就是只有在小程序后退、组件销毁、分享时才会触发

所以应该改变 `postMessage` 的时机，调换顺序就可以了



```html
<script type="text/javascript">
    wx.miniProgram.navigateBack({delta: 1})
    
    wx.miniProgram.postMessage({ data: '获取成功' })
</script>
```




## Exif.js

### 使用到的库
* [exif.js](http://code.ciaoca.com/javascript/exif-js/)
* [html2canvas](http://html2canvas.hertzen.com/)

### 将拍摄的图像绘制到 Canvas 后，图像方向不对
在微信中使用 `<input type="file" multiple accept="image/*">` 传入照片后，绘制到 Canvas 中时会发现绘制的图像方向不对（手 Q 端貌似不会存在这个问题），这时需要使用 exif.js 来解决。

> Exif.js 提供了 JavaScript 读取图像的原始数据的功能扩展，例如：拍照方向、相机设备型号、拍摄时间、ISO 感光度、GPS 地理位置等数据。


```html
<input class="uploadBtn" type="file" multiple accept="image/*">
```


```js
document.querySelector(".uploadBtn").addEventListener("change", previewImgFile, false);
 
function previewImgFile() {
 
    var _files = files || event.target.files;
    var _index = index || 0;
    var reader = new FileReader();
 
    reader.onload = function(event) {
 
        var image = new Image();
            image.src = event.target.result;
 
        var orientation;
 
        image.onload = function() {
 
            EXIF.getData(image, function() { // 获取图像的数据
 
                EXIF.getAllTags(this); // 获取图像的全部数据，值以对象的方式返回
                orientation = EXIF.getTag(this, "Orientation"); // 获取图像的拍摄方向
 
                var rotateCanvas = document.createElement("canvas"),
                    rotateCtx = rotateCanvas.getContext("2d");
 
                // 针对图像方向进行处理
                switch (orientation) {
 
                    case 1 :
                        rotateCanvas.width = image.width;
                        rotateCanvas.height = image.height;
                        rotateCtx.drawImage(image, 0, 0, image.width, image.height);
                        break;
                    case 6 : // 顺时针 90 度
                        rotateCanvas.width = image.height;
                        rotateCanvas.height = image.width;
                        rotateCtx.translate(0, 0);
                        rotateCtx.rotate(90 * Math.PI / 180);
                        rotateCtx.drawImage(image, 0, -image.height, image.width, image.height);
                        break;
                    case 8 :
                        rotateCanvas.width = image.height;
                        rotateCanvas.height = image.width;
                        rotateCtx.translate(0, 0);
                        rotateCtx.rotate(-90 * Math.PI / 180);
                        rotateCtx.drawImage(image, -image.width, 0, image.width, image.height);
                        break;
                    case 3 : // 180 度
                        rotateCanvas.width = image.width;
                        rotateCanvas.height = image.height;
                        rotateCtx.translate(0, 0);
                        rotateCtx.rotate(Math.PI);
                        rotateCtx.drawImage(image, -image.width, -image.height, image.width, image.height);
                        break;
                    default :
                        rotateCanvas.width = image.width;
                        rotateCanvas.height = image.height;
                        rotateCtx.drawImage(image, 0, 0, image.width, image.height);
 
                }
 
                var rotateBase64 = rotateCanvas.toDataURL("image/jpeg", 0.5);
 
            });
 
        }
 
    }
 
    reader.readAsDataURL(_files[_index]);
 
}
```



## Canvas 多行文本

```js
 CanvasRenderingContext2D.prototype.wrapText = function(text, x, y, maxWidth, lineHeight) {
 	if (typeof text != 'string' || typeof x != 'number' || typeof y != 'number') {
 		return;
 	}

 	var context = this;
 	var canvas = context.canvas;

 	if (typeof maxWidth == 'undefined') {
 		maxWidth = (canvas && canvas.width) || 300;
 	}
 	if (typeof lineHeight == 'undefined') {
 		lineHeight = (canvas && parseInt(window.getComputedStyle(canvas).lineHeight)) || parseInt(window.getComputedStyle(document.body).lineHeight);
 	}

 	// 字符分隔为数组
 	var arrText = text.split('');
 	var line = '';

 	for (var n = 0; n < arrText.length; n++) {
 		var testLine = line + arrText[n];
 		var metrics = context.measureText(testLine);
 		var testWidth = metrics.width;
 		if (testWidth > maxWidth && n > 0) {
 			context.fillText(line, x, y);
 			line = arrText[n];
 			y += lineHeight;
 		} else {
 			line = testLine;
 		}
 	}
 	context.fillText(line, x, y);
 };
```

## canvas 绘制图片模式


```js
/**
 * @param {Number} box_w 固定盒子的宽, box_h 固定盒子的高
 * @param {Number} source_w 原图片的宽, source_h 原图片的高
 * @return {Object} {截取的图片信息}，对应drawImage(imageResource, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)参数
 */

function coverImg(box_w, box_h, source_w, source_h) {
	var sx = 0,
		sy = 0,
		sWidth = source_w,
		sHeight = source_h;
	if (source_w > source_h || (source_w == source_h && box_w < box_h)) {
		sWidth = box_w * sHeight / box_h;
		sx = (source_w - sWidth) / 2;
	} else if (source_w < source_h || (source_w == source_h && box_w > box_h)) {
		sHeight = box_h * sWidth / box_w;
		sy = (source_h - sHeight) / 2;
	}
	return {
		sx, sy, sWidth, sHeight
	}
}


/**
 * @param {Number} sx 固定盒子的x坐标,sy 固定盒子的y左标
 * @param {Number} box_w 固定盒子的宽, box_h 固定盒子的高
 * @param {Number} source_w 原图片的宽, source_h 原图片的高
 * @return {Object} {drawImage的参数，缩放后图片的x坐标，y坐标，宽和高},对应drawImage(imageResource, dx, dy, dWidth, dHeight)
 */

function containImg(sx, sy, box_w, box_h, source_w, source_h) {
	var dx = sx,
		dy = sy,
		dWidth = box_w,
		dHeight = box_h;
	if (source_w > source_h || (source_w == source_h && box_w < box_h)) {
		dHeight = source_h * dWidth / source_w;
		dy = sy + (box_h - dHeight) / 2;

	} else if (source_w < source_h || (source_w == source_h && box_w > box_h)) {
		dWidth = source_w * dHeight / source_h;
		dx = sx + (box_w - dWidth) / 2;
	}
	return {
		dx, dy, dWidth, dHeight
	}
}
```


## canvas 图片压缩

```javascript
function zipImg(imgpath) {
    //开始canvas绘图
    var c = document.getElementById("myCanvas");
    var ctx = c.getContext("2d");
    //src赋值给临时的隐藏的图片
    var img = document.getElementById("HidImg");
    img.src = imgpath;
    //图片载入开始执行压缩
    img.onload = function() {
        var height = img.height;
        var width = img.width;
        //判断图片宽高
        var chu = 6;
        //如果图片宽度在4080-3656之间，5倍压缩宽高
        if (width <= 4080 && width >= 3656) {
            chu = 5;
        }
        //如果图片宽度在2500-1440之间，4.5倍压缩宽高 
        else if (width <= 3656 && width >= 2500) {
            chu = 4.5;
        }
        //如果图片宽度在2500-1440之间，3.5倍压缩宽高
        else if (width <= 2500 && width >= 1440) {
            chu = 3.5;
        }
        //如果图片宽度在1440-1080之间，2倍压缩宽高
        else if (width <= 1440 && width >= 1080) {
            chu = 2;
        }
        //如果图片宽度在1080-800之间，1.6倍压缩宽高
        else if (width <= 1080 && width > 800) {
            chu = 1.6;
        }
        //低于800不进行宽高压缩
        else if (width <= 800) {
            chu = 1
        }
        //获取倍率后开始宽高压缩
        img.height = height / chu;
        img.width = width / chu;
        c.width = width / chu;
        c.height = height / chu;
        //开始绘图
        ctx.drawImage(img, 0, 0, c.width, c.height);
        var ext = img.src.substring(img.src.lastIndexOf(".") + 1).toLowerCase();
        //转化成base64数据，记住一定要转化成jpeg格式，转化成png不能选择清晰度，并且base64长度
        //会特别长 括号里的0.5即为0.5的清晰度。0-1之间，越高越清晰(当然base64的体积也会越来越大)
        var dataURL = myCanvas.toDataURL("image/jpeg", 0.5);
        //由于base64转码属于异步操作，给1.2秒延迟，转码1.2秒后获取转码后的base64数
        //据(针对WebAPP，PC端可直接忽略)
        setTimeout(function() {
            $(".NewImg").attr("src", dataURL); //dataURL即为压缩后的图片
        },
        1200)
    }
}
```

## canvas 海报跨域

```javascript
var cans = document.getElementById('haibao');
var ctx = cans.getContext('2d');
cans.width = 750;
cans.height = 1206;

var resultimg = new Image();
//直接读成blob文件对象
var xhr = new XMLHttpRequest();
xhr.open('get', data.result, true);
xhr.responseType = 'blob';
xhr.onload = function () {
    if (this.status == 200) {
        //这里面可以直接通过URL的api将其转换，然后赋值给img.src
        //也可以使用下面的preView方法将其转换成base64之后再赋值
        resultimg.src = URL.createObjectURL(this.response);
    }
};
xhr.send();

resultimg.onload = function () {
    ctx.drawImage(resultimg, 0, 0);
    var weixin = new Image();
    weixin.src = './img/weixin.png';
    weixin.onload = function () {
        ctx.drawImage(weixin, 315, 865, 155, 155);
        var logo = new Image();
        logo.src = './img/logo.jpg?t1';
        logo.onload = function () {
            ctx.drawImage(logo, 265, 1057, 250, 99);
            setTimeout(function () {
                $(".page2").fadeOut(300);
                clearInterval(timers[1]);
                $(".page2 .text").empty();
                movieClip2.stop();
                $(".jieguoye").fadeIn(400);
                var can = document.getElementById('haibao');
                var imgsrc = can.toDataURL("image/png");
                $(".jieguo").attr("src", imgsrc)
            }, 100)
        }
    }
}
```


## 移动端资源总结

工作了有一段时间，基本上都在搞移动端的前端开发，工作的过程中遇到过很多问题，bug的解决方案，记录下来，以便后用！！！内容并不是很全，以后每遇到一个问题都会总结在这里，分享给大家！

一、meta标签相关知识

**1、移动端页面设置视口宽度等于设备宽度，并禁止缩放。**

```
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
```

**2、移动端页面设置视口宽度等于定宽（如640px），并禁止缩放，常用于微信浏览器页面。**

```
<meta name="viewport" content="width=640,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
```

**3、禁止将页面中的数字识别为电话号码**

```
<meta name="format-detection" content="telephone=no" />
```

**4、忽略Android平台中对邮箱地址的识别**

```
<meta name="format-detection" content="email=no" />
```

**5、当网站添加到主屏幕快速启动方式，可隐藏地址栏，仅针对ios的safari**

```
<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- ios7.0版本以后，safari上已看不到效果 -->
```

**6、将网站添加到主屏幕快速启动方式，仅针对ios的safari顶端状态条的样式**

```
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<!-- 可选default、black、black-translucent -->
```

viewport模板

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" name="viewport">
<meta content="yes" name="apple-mobile-web-app-capable">
<meta content="black" name="apple-mobile-web-app-status-bar-style">
<meta content="telephone=no" name="format-detection">
<meta content="email=no" name="format-detection">
<title>title</title>
<link rel="stylesheet" href="index.css">
</head>

<body>
    content...
</body>

</html>
```

二、CSS样式技巧

**1、禁止ios和android用户选中文字**

```
.css{-webkit-user-select:none}
```

**2、禁止ios长按时触发系统的菜单，禁止ios&android长按时下载图片**

```
.css{-webkit-touch-callout: none}
```

**3、webkit去除表单元素的默认样式**

```
.css{-webkit-appearance:none;}
```

**4、修改webkit表单输入框placeholder的样式**

```
input::-webkit-input-placeholder{color:#AAAAAA;}
input:focus::-webkit-input-placeholder{color:#EEEEEE;}
```

**5、去除android a/button/input标签被点击时产生的边框 & 去除ios a标签被点击时产生的半透明灰色背景**

```
a,button,input{-webkit-tap-highlight-color:rgba(255,0,0,0);}
```

**6、ios使用-webkit-text-size-adjust禁止调整字体大小**

```
body{-webkit-text-size-adjust: 100%!important;}
```

**7、android 上去掉语音输入按钮**

```
input::-webkit-input-speech-button {display: none}
```

**8、移动端定义字体，移动端没有微软雅黑字体**

```
/* 移动端定义字体的代码 */
body{font-family:Helvetica;}
```

三、其他技巧

**1、手机拍照和上传图片**

```
<!-- 选择照片 -->
<input type=file accept="image/*">
<!-- 选择视频 -->
<input type=file accept="video/*">
```

**2、取消input在ios下，输入的时候英文首字母的默认大写**

```
<input autocapitalize="off" autocorrect="off" />
```

**3、打电话和发短信**

```
<a href="tel:0755-10086">打电话给:0755-10086</a>
<a href="sms:10086">发短信给: 10086</a>
```

四、CSS reset

```
/* hcysun  */
@charset "utf-8";
/* reset */
html{
    -webkit-text-size-adjust:none;
    -webkit-user-select:none;
    -webkit-touch-callout: none
    font-family: Helvetica;
}
body{font-size:12px;}
body,h1,h2,h3,h4,h5,h6,p,dl,dd,ul,ol,pre,form,input,textarea,th,td,select{margin:0; padding:0; font-weight: normal;text-indent: 0;}
a,button,input,textarea,select{ background: none; -webkit-tap-highlight-color:rgba(255,0,0,0); outline:none; -webkit-appearance:none;}
em{font-style:normal}
li{list-style:none}
a{text-decoration:none;}
img{border:none; vertical-align:top;}
table{border-collapse:collapse;}
textarea{ resize:none; overflow:auto;}
/* end reset */
```

五、常用公用CSS style

```
/* public */

/* 清除浮动 */
.clear { zoom:1; }
.clear:after { content:''; display:block; clear:both; }

/* 定义盒模型为怪异和模型（宽高不受边框影响） */
.boxSiz{
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -ms-box-sizing: border-box;
    -o-box-sizing: border-box;
    box-sizing: border-box;
}

/* 强制换行 */
.toWrap{
    word-break: break-all;       /* 只对英文起作用，以字母作为换行依据。 */
    word-wrap: break-word;      /* 只对英文起作用，以单词作为换行依据。*/
    white-space: pre-wrap;     /* 只对中文起作用，强制换行。*/
}

/* 禁止换行 */
.noWrap{
    white-space:nowrap;
}

/* 禁止换行,超出省略号 */
.noWrapEllipsis{
     white-space:nowrap; overflow:hidden; text-overflow:ellipsis;
}

/* 文字两端对齐 */
.text-justify{
    text-align:justify; 
    text-justify:inter-ideograph;
}

/* 定义盒模型为 flex布局兼容写法并让内容水平垂直居中 */
.flex-center{
    display: -webkit-box;
    display: -moz-box;
    display: -ms-flexbox;
    display: -o-box;
    display: box;

    -webkit-box-pack: center;
    -moz-box-pack: center;
    -ms-flex-pack: center;
    -o-box-pack: center;
    box-pack: center;

    -webkit-box-align: center;
    -moz-box-align: center;
    -ms-flex-align: center;
    -o-box-align: center;
    box-align: center;
}

/* public end */
```

六、flex布局

**1、定义弹性盒模型兼容写法**

```
/*
    box
    inline-box
*/
display: -webkit-box;
display: -moz-box;
display: -ms-flexbox;
display: -o-box;
display: box;
```

**2、box-orient 定义盒模型内伸缩项目的布局方向**

```
/**
 * vertical column  垂直
 * horizontal row   水平 默认值
 */
-webkit-box-orient: horizontal;
-moz-box-orient: horizontal;
-ms-flex-direction: row;
-o-box-orient: horizontal;
box-orient: horizontal;
```

**3、box-direction 定义盒模型内伸缩项目的正序(normal默认值)、倒叙(reverse)**

```
/* Firefox */
display:-moz-box;
-moz-box-direction:reverse;
/* Safari、Opera 以及 Chrome */
display:-webkit-box;
-webkit-box-direction:reverse;
```

**4、box-pack 对盒子水平富裕空间的管理**

```
/*
    start
    end
    center
    justify
*/
-webkit-box-pack: center;
-moz-box-pack: center;
-ms-flex-pack: center;
-o-box-pack: center;
box-pack: center;
```

**5、box-pack 对盒子垂直方向富裕空间的管理**

```
/*
    start
    end
    center
*/
/* box-align */
-webkit-box-align: center;
-moz-box-align: center;
-ms-flex-align: center;
-o-box-align: center;
box-align: center;
```

**6、定义伸缩项目的具体位置**

```
/*-moz-box-ordinal-group:1;*/ /* Firefox */
/*-webkit-box-ordinal-group:1;*/ /* Safari 和 Chrome */
.box div:nth-of-type(1){-webkit-box-ordinal-group:1;}
.box div:nth-of-type(2){-webkit-box-ordinal-group:2;}
.box div:nth-of-type(3){-webkit-box-ordinal-group:3;}
.box div:nth-of-type(4){-webkit-box-ordinal-group:4;}
.box div:nth-of-type(5){-webkit-box-ordinal-group:5;}
```

**7、定义伸缩项目占空间的份数**

```
-moz-box-flex:2.0; /* Firefox */
-webkit-box-flex:2.0; /* Safari 和 Chrome */

.box div:nth-of-type(1){-webkit-box-flex:1;}
.box div:nth-of-type(2){-webkit-box-flex:2;}
.box div:nth-of-type(3){-webkit-box-flex:3;}
.box div:nth-of-type(4){-webkit-box-flex:4;}
.box div:nth-of-type(5){-webkit-box-flex:5;}
```

## 优秀的博客

> 前端收集

在前端路上摸索前行，在这里分享自己长期关注的前端开发相关的优秀网站、博客、以及活跃开发者。欢迎更新，以下各排名不分先后顺序。

自己 RSS 长期订阅了一些IT 和技术相关博客，这里是我Feedly 输出的opml，可直接导入一些RSS 阅读器:
[https://github.com/foru17/luolei-dotfiles/blob/master/feedly.opml](https://github.com/foru17/luolei-dotfiles/blob/master/feedly.opml)

### [前端收集图谱](http://get-set.cn/front-end-collect/)

此部分为[@jikeytang ](https://github.com/jikeytang)贡献

- clone https://github.com/foru17/front-end-collect
- cd front-end-collect
- bower install
- 放入你喜欢的web容器,访问index.html即可
- 你也直接可以访问: [https://front-end-collect.is26.com](https://front-end-collect.is26.com)
- 支持Chrome, Firefox and IE10&11以上浏览器

### 聚合&&周报订阅

| 名称                         | 订阅地址                           | 介绍                               |
| ---------------------------- | ---------------------------------- | ---------------------------------- |
| **英文推送**                 |                                    |                                    |
| Html5 Weekly                 | http://html5weekly.com/            | Html 技术类                        |
| CSS Weekly                   | http://css-weekly.com/             |                                    |
| Javascript Weekly            | http://javascriptweekly.com/       | JS相关，同样有 html,css 和工具相关 |
| Web Design Weekly            | http://web-design-weekly.com/      | 设计、技术、技巧、工具聚合         |
| UX Weekly                    | http://uxwkly.com/                 | 用户体验、网页设计推送             |
| Web Tools Weekly             | http://webtoolsweekly.com/         | Js，工具推送                       |
| RESPONSIVE DESIGN NEWSLETTER | http://responsivedesignweekly.com/ | 每周推送一次响应式设计相关         |
| Tutorialzine                 | http://tutorialzine.com/           | 精品教程和资源推送                 |
| Sidebar                      | http://sidebar.io/                 | 每天、每半周、每周推送5条设计相关  |
| The Hacker News Newsletter   | http://www.hackernewsletter.com/   | HN 每周精选                        |
| Design News                  | https://news.layervault.com/       | F2类资讯聚合                       |

|Css Animations|http://cssanimation.rocks/|关于CSS动画的订阅
|HACKDESIGN|http://hackdesign.org/|每周发布一个设计类课程|
|**中文推送**|||
|稀土:掘金|http://gold.xitu.io/|国内十分用心的开发者技术分享、交流平台|
|SegmentFault精选 |http://segmentfault.com/|国内开发者技术问答社区每周精选问答|
|FE Weekly|http://www.feweekly.com/|每周一次，内容主要是英文的，不过有中文导读
|EchoJs_News|http://www.echojs.com/|每天推送若干好文，都是英文的，JS技术类|
|?碼天狗週刊 |http://weekly.codetengu.com/|台湾的，一份開發者導向的IT 技術週刊，適合所有患有資訊焦慮症、氣血循環不順以及性受挫的軟體工程師們。|
|Github Issue Blog Reader|https://gitissue.com|Github Issue 博客聚合平台，每天更新内容，可及时关注阅读|

### 专业博客

注:此处`活跃度`为博客更新频率，`原创度`指的是作者原创或者翻译的文章所占博文比例。请尊重原创，大量转载其他网站资讯的网站和聚合类网站不做推荐。

### 中文博客

| 名称                                                         | 活跃度 | 原创度 | 维护者                                                       | 其他                                             |
| ------------------------------------------------------------ | ------ | ------ | ------------------------------------------------------------ | ------------------------------------------------ |
| [W3Cplus](http://www.w3cplus.com/)                           | ★★★★★  | ★★★★★  | 携程 @大漠                                                   | 国内最优秀的前端博客，原创居多                   |
| [W3Cfuns](http://www.w3cfuns.com/)                           | ★★★★★  | ★★★★☆  | [#](http://www.w3cfuns.com/misc.php?mod=faq&action=faq&id=1) | 专注于web前端开发行业的综合性门户网站            |
| [前端观察](http://www.qianduan.net/)                         | ★★★★☆  | ★★★★☆  | 腾讯 ISUX @神飞                                              | 曾经最优秀，最近更新不频繁了                     |
| [腾讯web前端 AlloyTeam 团队](http://www.alloyteam.com/)      | ★★★★   | ★★★★   | [@腾讯AlloyTeam](http://t.qq.com/AlloyTeam)                  | 来自于腾讯SNG(社交网络事业群)                    |
| [张鑫旭-鑫空间-鑫生活](http://www.zhangxinxu.com/wordpress/) | ★★★★☆  | ★★★★★  | 张鑫旭                                                       | 重构很厉害，不少经典文章经验                     |
| [ria之家](http://www.36ria.com/)                             | ★★★★☆  | ★★★★☆  | 淘宝 @明河                                                   | #                                                |
| [大前端](http://www.daqianduan.com/)                         | ★★★★☆  | ★★★★☆  | [#](http://www.cssforest.org/blog/index.php?s=about)         | #                                                |
| [CSS森林](http://www.cssforest.org/blog/)                    | ★★★★☆  | ★★★★☆  | [关于](http://www.cssforest.org/blog/index.php?s=about)      | #                                                |
| [设计达人](http://www.shejidaren.com/)                       | ★★★★☆  | ★★★☆☆  | [#](http://www.cssforest.org/blog/index.php?s=about)         | 更新较频繁，但转载也较多                         |
| [阮一峰博客](http://www.ruanyifeng.com/blog/)                | ★★★★☆  | ★★★☆☆  | [#](http://www.ruanyifeng.com/about.html)                    | 牛人一个                                         |
| [Be For Web - 为网而生 - 原创译文博客](http://beforweb.com/) | ★★★★☆  | ★★★★☆  | [@C7210](http://weibo.com/c7210)                             | 关注移动应用及互联网产品、用户体验设计、前端开发 |

### 国外博客

| 名称                                                  | 活跃度 | 原创度 | 维护者            | 其他                          |
| ----------------------------------------------------- | ------ | ------ | ----------------- | ----------------------------- |
| [Smashing Magazine](http://www.smashingmagazine.com/) | ★★★★★  | ★★★★★  | #                 | 业界权威，web 设计很赞        |
| [Tuts](http://hub.tutsplus.com/)                      | ★★★★★  | ★★★★★  | -                 | 国外知名开发者网站            |
| [DeveloperDrive](http://www.developerdrive.com/)      | ★★★★★  | ★★★★★  | -                 | 优质前端技术信息              |
| [CSS-TRICKS](http://css-tricks.com/)                  | ★★★★★  | ★★★★★  | Chris Coyier      | 左边这位是大神                |
| [Web Designer Wall](http://webdesignerwall.com/)      | ★★★★★  | ★★★★★  | Nick La.          | 优质 Html5,CSS3等教程         |
| [Tutorialzine](http://tutorialzine.com/)              | ★★★★★  | ★★★★★  | #                 | 大量 web 教程和资源           |
| [Inspect Element](http://inspectelement.com/)         | ★★★★★  | ★★★★★  | #                 | CSS,wordpress 相关教程挺多    |
| [Codrops](http://tympanus.net/codrops/)               | ★★★★★  | ★★★★★  | #                 | 设计、交互、CSS               |
| [Jake Rutter](http://www.onerutter.com/)              | ★★★★★  | ★★★★★  | Jake Rutter       | Jquery 作者，不解释了         |
| [Paul Irish](http://www.paulirish.com/)               | ★★★★★  | ★★★★★  | Paul Irish        | 大神,Google Chrome团队,Yeoman |
| [Krasimir Tsonev](http://krasimirtsonev.com/blog)     | ★★★★★  | ★★★★★  | Krasimir Tsonev   | html5,ccs3,javascript         |
| [NCZOnline](http://www.nczonline.net/)                | ★★★★★  | ★★★★★  | Nicholas C. Zakas | html5,ccs3,javascript         |
| [HTML5 Rocks](http://www.html5rocks.com/en/)          | ★★★★★  | ★★★★★  | #                 | html5权威网站                 |
| [A List Apart](http://alistapart.com/)                | ★★★★★  | ★★★★★  | #                 | 可以改变世界的文章            |
| [hakim](http://hakim.se/)                             | ★★★★★  | ★★★★★  | HAKIM EL HATTAB   | ccs3,javascript               |
| [DailyJS](http://dailyjs.com/)                        | ★★★★★  | ★★★★★  | #                 | javascript                    |

### 活跃微博

| ID                                             | 公司     | 简介                                           |
| ---------------------------------------------- | -------- | ---------------------------------------------- |
| [@稀土圈](http://weibo.com/xitucircle)         | #        | 强烈推荐，分享一些技术文章和Github项目         |
| [@w3c中国](http://weibo.com/w3cchina)          | #        | 万维网联盟中国办事处官方微博                   |
| [@TheFrontEnd](http://weibo.com/javascriptdev) | #        | JavaScript技术资讯、新闻、教程、深度文章。     |
| [@前端快爆](http://weibo.com/fekb)             | 阿里巴巴 | 有HTML5、CSS3、JS                              |
| [@HTML5中国](http://e.weibo.com/html5cn)       | #        | 中国www.html5cn.org官方微博                    |
| [@GitHubDaily](http://e.weibo.com/githubdaily) | #        | 专注于分享 GitHub 最新的优质开源项目:sparkles: |

### 开发者博客

微博微信流行后，明显感觉到写博客的人还是越来越少了，下面推荐的这些开发者属于在网上比较活跃的，或者博客积累了大量优质资源的。

### 国内开发者

国内开发者一块欢迎大家 `Fork`提交推荐，最好能推荐一些在前端界较活跃的的开发者。

| ID         | 博客                                               | 微博                                         | Github                                        | Twitter                                          | 公司                     | 关键字                                         |
| ---------- | -------------------------------------------------- | -------------------------------------------- | --------------------------------------------- | ------------------------------------------------ | ------------------------ | ---------------------------------------------- |
| 阮一峰     | [阮一峰博客](http://www.ruanyifeng.com/blog/)      | [@ruanyf](http://weibo.com/ruanyf)           | [@ruanyf](https://github.com/ruanyf)          | [@ruanyf](https://twitter.com/ruanyf)            | 上海金融学院国际金融学院 | 教师，博客写作人，翻译人，《黑客与画家》的译者 |
| 老赵       | http://blog.zhaojie.me/                            | [@老赵](http://weibo.com/jeffz)              | #                                             | [#]()                                            | 摩根大通（香港）         | 资深码农                                       |
| 玉伯       | [岁月如歌](http://lifesinger.wordpress.com/)       | [@玉伯也叫射雕](http://weibo.com/lifesinger) | [@lifesinger](https://github.com/lifesinger)  | [@lifesinger](https://twitter.com/lifesinger)    | 支付宝                   | 大牛                                           |
| kejun      | http://hikejun.com/                                | [@kejunz](http://weibo.com/kejunz)           | [@kejunz](https://github.com/kejun)           | #                                                | 豆瓣                     | 前端大神                                       |
| 寒冬winter | [winter-cn](http://winter-cn.cnblogs.com/)         | [@寒冬winter](http://weibo.com/wintercn)     | #                                             | #                                                | #                        | #                                              |
| 左耳朵耗子 | [酷壳](http://coolshell.cn/)                       | [@左耳朵耗子](http://weibo.com/haoel)        | #                                             | [@haoel](https://twitter.com/haoel)              | 淘宝                     | #                                              |
| fool2fish  | #                                                  | [@fool2fish](http://weibo.com/fool2fish)     | #                                             | #                                                | 支付宝                   | #                                              |
| 朴灵       | [Html5fiy](http://html5ify.com/)                   | [@朴灵](http://weibo.com/shyvo)              | [JacksonTian](https://github.com/JacksonTian) | #                                                | 阿里巴巴                 | 《深入浅出Node.js》作者,大牛                   |
| Cat Chen   | [陈广琛](https://catchen.me/)                      | [@CatChen](http://weibo.com/u/1640352230)    | [@CatChen](https://github.com/CatChen)        | [@CatChen](https://twitter.com/CatChen)          | Facebook                 | 大牛                                           |
| BYVoid     | [Beyond the Void](https://www.byvoid.com/)         | [@BYVoid](http://weibo.com/byvoid)           | [@BYVoid](https://github.com/BYVoid)          | [@BYVoid](https://twitter.com/byvoid)            | Facebook 英国            | 《Node.js 开发指南》作者,大牛                  |
| 勾三股四   | #                                                  | [@勾三股四](http://weibo.com/mx006)          | #                                             | #                                                | 淘宝                     | #                                              |
| cnberg     | [冰山一角](http://cnberg.com)                      | [@berg](http://weibo.com/berg)               | @cnberg                                       | [@cnberg]()                                      | 百度                     | 骑行                                           |
| 小胡子哥   | [barretlee](http://www.barretlee.com/)             | [@Barret李靖](http://weibo.com/173248656)    | [@barretlee](https://github.com/barretlee)    | #                                                | #                        | 阿里巴巴                                       |
| 小鱼       | [sofish](http://sofish.de/)                        | [@sofish](http://weibo.com/sofish)           | #                                             | #                                                | #                        | 饿了么前端                                     |
| 屈光宇     | [Jerry Qu的小站](https://imququ.com/)              | [屈光宇](http://weibo.com/jerryqu)           | #                                             | #                                                | #                        | 奇虎360前端,HTTP,Node。js                      |
| 郭宇       | [Einmal ist keinmal](http://blog.guoyu.me/)        | [@郭宇](http://weibo.com/137601206)          | [@guo-yu](https://github.com/guo-yu)          | [@turingou](https://twitter.com/turingou)        | 今日头条                 | Node.js                                        |
| 张鑫旭     | [张鑫旭博客](http://www.zhangxinxu.com/wordpress/) | [@张鑫旭](http://weibo.com/zhangxinxu)       | [@zhangxinxu](https://github.com/zhangxinxu)  | [@zhangxinxu](https://twitter.com/zhangxinxu)    | 阅文(腾讯文学) YUED      | 前端开发                                       |
| 罗磊       | [罗磊的独立博客](https://luolei.org)               | [@罗罗磊磊](http://weibo.com/foru17)         | [@foru17](https://github.com/foru17)          | [@foru17](https://twitter.com/luoleiorg)         | 阅文(腾讯文学) YUED      |                                                |
| hzlzh      | [自力博客](https://zlz.im)                         | [@hzlzh](http://weibo.com/hzlzh)             | [@hzlzh](http://github.com/hzlzh)             | [@hzlzh](http://twitter.com/hzlzh)               | 腾讯                     | 前端开发                                       |
| TQ         | http://targetkiller.net/                           | [@Piser-TQ](http://weibo.com/targetkiller)   | [@tqtan](https://twitter.com/tqtan/)          | [@targetkiller](https://github.com/targetkiller) | 腾讯 微信                | 前端                                           |
| LOO2K      | [LOO2K](http://loo2k.com/blog/)                    | [@LOO2K](http://weibo.com/loo2k)             | [LOO2K](https://github.com/loo2k)             | [LOO2K](https://twitter.com/loo2k/)              | 腾讯 CDC                 | 前端                                           |
| 大猫       | [意淫笔记](http://bigc.at)                         | [@daemao](http://weibo.com/daemao)           | [@Damao](https://github.com/Damao)            | [@13igcat](https://twitter.com/13igcat)          | 腾讯                     | [知乎](http://www.zhihu.com/people/13igcat)    |
| C7210      | beforweb.com/                                      | [@C7210](http://weibo.com/c7210)             | [@C7210](http://twittercom/hzlzh)             | [@C7210](http://github.com/hzlzh)                | #                        | UX、交互设计师、视觉与前端                     |
| kejun      | http://hikejun.com/                                | [#](http://weibo.com/kejun)                  | [#](http://twittercom/kejun)                  | [#](http://github.com/hzlzh)                     | 腾讯                     | 前端开发                                       |
| lucifr     | http://lucifr.com/                                 | [@lucifr](http://weibo.com/lucifr)           | [@lucifr](http://twittercom/lucifr)           | [@lucifr](http://github.com/lucifr)              | #                        | Mac,ios                                        |
| smallni    | http://www.smallni.com/                            | [#](http://weibo.com/hzlzh)                  | [@Smallni](https://twitter.com/smallniding/)  | [#](http://github.com/hzlzh)                     | 腾讯                     | 前端开发                                       |
| qiqiboy    | [qiqiboy](http://www.qiqiboy.com/)                 | [@qiqiboy](http://weibo.com/qiqiboy)         | #                                             | #                                                | 老虎证券                 | 吐槽清理大师开发者                             |
| 周爱民     | [aimingoo专栏](http://blog.csdn.net/aimingoo/)     | #                                            | #                                             | #                                                | 支付宝                   | JavaScript语言精髓与编程实践作者               |
| 李松峰     | [为之漫笔](http://www.cn-cuckoo.com)               | #                                            | #                                             | #                                                | #                        | 高程2等书的译者                                |
| 99css      | [99css](http://www.99css.com/)                     | [@ytzong](http://weibo.com/ytzong)           | #                                             | #                                                | #                        | 腾讯一牛                                       |
| 秦歌       | [Kaven](http://dancewithnet.com/)                  | #                                            | [@kavenyan](http://twitter.com/kavenyan)      | #                                                | #                        | js语言精粹译者                                 |
| linxz      | [linxz](http://www.linxz.de/)                      | #                                            | #                                             | #                                                | #                        | css那些事儿的作者                              |
| Along      | [Along's Blog](http://jinlong.github.io/)          | [@newwave](http://weibo.com/newwave)         | #                                             | #                                                | #                        | Opera 欧朋一牛                                 |
| 安记       | [cssha](http://www.cssha.com/)                     | [@hanan321](http://weibo.com/hanan321)       | [hanan198501](https://github.com/hanan198501) | #                                                | #                        | 去哪网一牛                                     |

| 余弦 | [EVILCOS](http://evilcos.me/) | [余弦](http://weibo.com/evilcos) | [evilcos](https://github.com/evilcos) | # | [知道创宇](http://www.knownsec.com/) | 安全（黑客）、架构、团队的各种观点与分享 | # | [冯大辉](http://dbanotes.net/) | 现在就职于丁香园 (http://dxy.cn) ，担任技术团队负责人.

### 一些社区

| 名称             | 地址                              | 介绍                                   |
| ---------------- | --------------------------------- | -------------------------------------- |
| V2EX             | http://v2ex.com/                  | 小众活跃社区                           |
| 稀土掘金         | https://juejin.im/                | 程序员同性交友社区                     |
| 知乎             | http://www.zhihu.com/             | 综合问答社区                           |
| 前端乱炖         | http://www.html-js.com/           | 专业的前端知识平台                     |
| segmentfault     | http://segmentfault.com/          | 综合问答社区                           |
| 果壳问答         | http://www.guokr.com/ask/pending/ | 综合问答社区                           |
| Ruby             | http://ruby-china.org/            | 同 V2EX 氛围类似，不局限于Ruby         |
| Node.js 中文社区 | http://cnodejs.org/               | Node.js 国内最活跃的社区               |
| Code Wall        | https://coderwall.com/            | 国外技术社区                           |
| DIV.IO           | http://div.io/                    | 国内前端技术社区                       |
| w3ctech          | http://www.w3ctech.com/           | 国内前端技术社区，常有一些线下活动发布 |

### 企业官方博客

在开头我的 Feedly 订阅 opml 文件里比较全面。

| 名称                                                         | 公司     | 部门 | 活跃度 | 简介                                                         | 微博                                      |
| ------------------------------------------------------------ | -------- | ---- | ------ | ------------------------------------------------------------ | ----------------------------------------- |
| [ISUX 社交用户体验设计](http://isux.tencent.com/)            | 腾讯     | ISUX | ★★★★☆  | 负责腾讯的社交网络相关产品的用户体验设计与研究。             | #                                         |
| [腾讯 CDC](http://cdc.tencent.com/)                          | 腾讯     | CDC  | ★★★★☆  | 简介                                                         | #                                         |
| [腾讯Web前端 Alloy 团队 Blog](http://www.alloyteam.com/)     | 腾讯     | SNG  | ★★★★☆  | 主要负责手机QQ、QQ互联、腾讯Q+、WebQQ项目的团队。            | [alloyteam](http://weibo.com/alloyteam)   |
| [TID-财付通设计中心](http://tid.tenpay.com/)                 | 腾讯     | TID  | ★★★★☆  | 简介                                                         | #                                         |
| [腾讯MXD移动互联网设计中心](http://mxd.tencent.com/)         | 腾讯     | MXD  | ★★★★☆  | 简介                                                         | [@腾讯MXD](http://e.t.qq.com/tencent_mxd) |
| [凹凸实验室](https://aotu.io/)                               | 京东     | FED  | ★★★★★  | 简凹凸实验室(Aotu.io，英文简称O2) 始建于2015年10月，是一个年轻基情的技术团队。 | #                                         |
| [微博UDC](http://udc.weibo.com/)                             | 新浪     | UDC  | ★★★★☆  | 简介                                                         | [@微博UDC设计中心](http://weibo.com/sudc) |
| [新浪UED](http://ued.sina.com.cn/)                           | 新浪     | UED  | ★★★★☆  | 简介                                                         | [#](http://weibo.com/sudc)                |
| [网易用户体验设计中心](http://uedc.163.com/)                 | 网易     | UED  | ★★★★☆  | 简介                                                         | [#](http://weibo.com/sudc)                |
| [阿里巴巴（中国站）用户体验设计部博客](http://www.aliued.cn/) | 阿里巴巴 | UED  | ★★★★☆  | 简介                                                         | [@Alibaba-UED](http://weibo.com/aliued)   |
| [携程UED-携程旅行前端开发团队](http://ued.ctrip.com/blog/)   | 携程网   | UED  | ★★★☆☆  | 携程UED,携程前端开发团队,UED,Javascript,重构,ux              | #                                         |
| [百度FEX](http://fex.baidu.com/)                             | 百度     | FEX  | ★★★★☆  | 百度前端团队Blog,关注前端技术，还更重视全端及全栈的能力。    | #                                         |
| [淘宝UED](http://ued.taobao.org/blog/)                       | 淘宝网   | UED  | ★★★★☆  | 用户体验、交互设计、视觉设计、前端技术博客                   | [@淘宝UED](http://weibo.com/taobaoued)    |

### 书籍

| 名称                                                         | 作者                           | 价格       | 出版社         | 简评                                                         |
| ------------------------------------------------------------ | ------------------------------ | ---------- | -------------- | ------------------------------------------------------------ |
| [Web标准设计](http://book.douban.com/subject/3327829/)       | 刘杰（嗷嗷）                   | RMB 60.00  | 清华大学出版社 | 基础入门                                                     |
| [大巧不工 : Web前端设计修炼之道](http://book.douban.com/subject/4914146/) | 赖定清 / 林坚                  | RMB 59.00  | 机械工业出版社 | 适合入门，了解前端全局                                       |
| [高性能网站建设指南:前端工程师技能精髓](http://book.douban.com/subject/3132277/) | Steve Souders                  | RMB 35.00  | 电子工业出版社 | 能从原理层理解各种方法                                       |
| [高性能网站建设指南:Web开发者性能优化最佳实践](http://book.douban.com/subject/4719162/) | Steve Souders                  | RMB 49.80  | 电子工业出版社 | #                                                            |
| [Web站点优化 : Web站点优化](http://book.douban.com/subject/4124141/) | 金                             | RMB 55.00  | #              | #                                                            |
| [Node.js开发指南](http://book.douban.com/subject/10789820/)  | 郭家寶                         | RMB 45.00  | #              | 作者很牛                                                     |
| [JavaScript高级程序设计](http://book.douban.com/subject/10546125/) | Nicholas C. Zakas              | RMB 99.00  | 人民邮电出版社 | 适合没事就翻翻                                               |
| [JavaScript权威指南](http://book.douban.com/subject/2228378/) | 弗拉纳根                       | RMB 109.00 | 机械工业出版社 | 犀牛书                                                       |
| [JavaScript语言精粹](http://book.douban.com/subject/3590768/) | Douglas Crockford              | RMB 35.00  | 电子工业出版社 | 绝对经典，相信看完后，对Javascript这门语言有了重新认识，原来这个语言是这么的美丽！ |
| [深入浅出node.js](http://book.douban.com/subject/25768396/)  | 朴灵                           | RMB 69.00  | 人民邮电出版社 | 一本从前端通往全端的好书                                     |
| [CSS开发王](http://book.douban.com/subject/3137282/)         | 张亚飞                         | RMB 49.00  | 电子工业出版社 | 适合有一定基础后CSS进阶用                                    |
| [JavaScript DOM编程艺术](http://book.douban.com/subject/6038371/) | Jeremy Keith /Jeffrey Sambells | RMB 49.00  | 人民邮电出版社 | 适合Javascript入门看                                         |

### 线上文档参考

| 书名                   | 地址                                             | 作者                 | 译者                          | 介绍                                                      |
| ---------------------- | ------------------------------------------------ | -------------------- | ----------------------------- | --------------------------------------------------------- |
| JavaScript秘密花园     | http://bonsaiden.github.io/JavaScript-Garden/zh/ | 伊沃·韦特泽尔&张易江 | [三生石上](http://sanshi.me/) | 完整书籍，界面美观，有详细demo                            |
| Material Design 中文版 | http://design.1sters.com/                        | Google设计手册       | 协同翻译                      | Google I/O 2014 发布的 Material Design 官方手册的中文翻译 |


## rem.js 适配

```js
<!-- rem -->
<script type="text/javascript">
    window.onload = function () {
    	getRem(750, 100)
    };
    window.onresize = function () {
    	getRem(750, 100)
    };
    getRem(750, 100);
    function getRem(pwidth, prem) {
    	var html = document.getElementsByTagName("html")[0];
    	var oWidth = document.body.clientWidth || document.documentElement.clientWidth;
    	html.style.fontSize = oWidth / pwidth * prem + "px";
	}
</script>
```

```js
//designWidth:设计稿的实际宽度值，需要根据实际设置
//maxWidth:制作稿的最大宽度值，需要根据实际设置
//这段js的最后面有两个参数记得要设置，一个为设计稿实际宽度，一个为制作稿最大宽度，例如设计稿为750，最大宽度为750，则为(750,750)
;(function(designWidth, maxWidth) {
	var doc = document,
	win = window,
	docEl = doc.documentElement,
	remStyle = document.createElement("style"),
	tid;

	function refreshRem() {
		var width = docEl.getBoundingClientRect().width;
		maxWidth = maxWidth || 540;
		width>maxWidth && (width=maxWidth);
		var rem = width * 100 / designWidth;
		remStyle.innerHTML = 'html{font-size:' + rem + 'px;}';
	}

	if (docEl.firstElementChild) {
		docEl.firstElementChild.appendChild(remStyle);
	} else {
		var wrap = doc.createElement("div");
		wrap.appendChild(remStyle);
		doc.write(wrap.innerHTML);
		wrap = null;
	}
	//要等 wiewport 设置好后才能执行 refreshRem，不然 refreshRem 会执行2次；
	refreshRem();

	win.addEventListener("resize", function() {
		clearTimeout(tid); //防止执行两次
		tid = setTimeout(refreshRem, 300);
	}, false);

	win.addEventListener("pageshow", function(e) {
		if (e.persisted) { // 浏览器后退的时候重新计算
			clearTimeout(tid);
			tid = setTimeout(refreshRem, 300);
		}
	}, false);

	if (doc.readyState === "complete") {
		doc.body.style.fontSize = "16px";
	} else {
		doc.addEventListener("DOMContentLoaded", function(e) {
			doc.body.style.fontSize = "16px";
		}, false);
	}
})(640, 640);
```

## ydui_flexible适配

```js
/**
 * YDUI 可伸缩布局方案
 * rem计算方式：设计图尺寸px / 100 = 实际rem  例: 100px = 1rem
 */
!function (window) {

    /* 设计图文档宽度 */
    var docWidth = 750;

    var doc = window.document,
        docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize';

    var recalc = (function refreshRem () {
        var clientWidth = docEl.getBoundingClientRect().width;

        /* 8.55：小于320px不再缩小，11.2：大于420px不再放大 */
        docEl.style.fontSize = Math.max(Math.min(20 * (clientWidth / docWidth), 11.2), 8.55) * 5 + 'px';

        return refreshRem;
    })();

    /* 添加倍屏标识，安卓倍屏为1 */
    docEl.setAttribute('data-dpr', window.navigator.appVersion.match(/iphone/gi) ? window.devicePixelRatio : 1);

    if (/iP(hone|od|ad)/.test(window.navigator.userAgent)) {
        /* 添加IOS标识 */
        doc.documentElement.classList.add('ios');
        /* IOS8以上给html添加hairline样式，以便特殊处理 */
        if (parseInt(window.navigator.appVersion.match(/OS (\d+)_(\d+)_?(\d+)?/)[1], 10) >= 8)
            doc.documentElement.classList.add('hairline');
    }

    if (!doc.addEventListener) return;
    window.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);

}(window);

```

## 常用插件

### 综合

- [Dowebok](http://www.dowebok.com/)
- [KitJs](http://xueduany.github.io/KitJs/KitJs/index.html)
- [Effeckt](http://h5bp.github.io/Effeckt.css/)

### 触屏滚动

- [TouchSlide](http://www.superslide2.com/TouchSlide/index.html)
- [Swiper](http://www.swiper.com.cn/)
- [Owlcarousel](http://owlgraphic.com/owlcarousel/)
- [Superslide2](http://www.superslide2.com/)
- [iSlider](http://be-fe.github.io/iSlider/)
- [FullPage](http://alvarotrigo.com/fullPage/)
- [Zepto.fullpage](https://github.com/yanhaijing/zepto.fullpage)

### 日期选择器

- [jquery-frontier-calendar](https://code.google.com/p/jquery-frontier-calendar/)
- [datetimepicker](http://www.bootcss.com/p/bootstrap-datetimepicker/)
- [pickadate](http://amsul.ca/pickadate.js/)
- [flipclockjs](http://flipclockjs.com/)
- [jq-timeTo](http://lexxus.github.io/jq-timeTo/)
- [glDatePicker](http://glad.github.io/glDatePicker/)
- [calendarsPicker](http://keith-wood.name/calendarsPicker.html)

### 图表

- [Echarts](http://echarts.baidu.com/)
- [Highcharts(中)](http://www.hcharts.cn/)
- [Highcharts(英)](http://www.highcharts.com/)
- [Jscharts](http://www.jscharts.com/)
- [Chart](http://www.bootcss.com/p/chart.js/)
- [Taucharts](http://taucharts.com/)

### 弹出层

- [sweetalert](http://t4t5.github.io/sweetalert/)
- [animatedModal](http://joaopereirawd.github.io/animatedModal.js/)
- [layer](http://layer.layui.com/)

### 其他

- [下拉刷新、上拉加载](https://github.com/ximan/dropload)
- [懒加载](https://github.com/tuupola/jquery_lazyload)
- [textarea自适应高度](https://github.com/phoetry/textareaAutoHeight)
- [多行文本溢出clamp-js](http://joe.sh/clamp-js)
- [多行文本溢出dotdotdot](http://dotdotdot.frebsite.nl/)
- [Letter-by-Letter](https://github.com/html5andblog/Letter-by-Letter-JS)
- [html5shiv(低版本浏览器的HTML5支持)](https://github.com/aFarkas/html5shiv)
- [jquery-placeholder](https://github.com/mathiasbynens/jquery-placeholder)
- [Js复制](http://zeroclipboard.org/)


## 常用框架库
### UI

- [Bootstrap(英)](http://getbootstrap.com/)
- [Bootstrap(中)](http://www.bootcss.com/)
- [Yui](http://yuilibrary.com/)
- [Foundation](http://foundation.zurb.com/index.html)
- [Jeasyui](http://www.jeasyui.net/)
- [Metroui](http://metroui.org.ua/)
- [Semantic](http://semantic-ui.com/)
- [Purecss](http://purecss.io/grids/)
- [Basscss](http://www.basscss.com/)
- [Jqueryui](http://jqueryui.com/)
- [Css3lib](http://css3lib.alloyteam.com/)
- [Uikit](http://www.getuikit.net/)

### js

- [Jquery](http://jquery.com/)
- [Zeptojs](http://www.zeptojs.cn/)
- [Angularjs(英)](https://angularjs.org/)
- [Angularjs(中)](https://www.angular.cn/)
- [Angularjs社区](http://angularjs.cn/)
- [React(英)](https://facebook.github.io/react/)
- [React(中)](http://react-china.org/)
- [Jslite](http://jslite.io/)
- [validate表单验证](https://github.com/rickharrison/validate.js)
- [Vuejs官方论坛](http://forum.vuejs.org/)
- [Backbonejs](http://backbonejs.org/)
- [Avalonjs](http://avalonjs.github.io/)
- [Thinkjs](https://thinkjs.org/)
- [Expressjs](http://expressjs.com/)
- [Seajs](http://seajs.org/)

### 动画库

- [Animate](http://daneden.github.io/animate.css/)
- [Anicollection](http://anicollection.github.io/)
- [Cssshake](http://elrumordelaluz.github.io/csshake/)
- [Animatable](http://leaverou.github.io/animatable/)
- [Hover](http://ianlunn.github.io/Hover/)
- [Animations](http://www.justinaguilar.com/animations/)
- [JXAnimate](http://alloyteam.github.io/JXAnimate/jxanimate_demo.html)
- [Spinkit](http://tobiasahlin.com/spinkit/)
- [Velocity动画](http://julian.com/research/velocity/)
- [AlloyStick骨骼动画引擎](http://alloyteam.github.io/AlloyStick/)
- [Rocket](http://minimamente.com/example/rocket/)
- [Cssynth](http://bennettfeely.com/cssynth/)
- [Stylie](http://jeremyckahn.github.io/stylie/)
- [Animo-js](http://labs.bigroomstudios.com/libraries/animo-js)
- [Anima](http://lvivski.com/anima/)
- [Magic_animations](http://www.minimamente.com/example/magic_animations/)
- [Dynamicsjs](http://dynamicsjs.com/)
- [Anijs](http://anijs.github.io/)
- [Motio](http://darsa.in/motio/)
- [Snabbt](https://daniel-lundin.github.io/snabbt.js/)
- [Textillate](https://jschr.github.io/textillate/)
- [Animsition](http://git.blivesta.com/animsition/)
- [Parallax](http://matthew.wagerfield.com/parallax/)
- [FakeLoader](http://joaopereirawd.github.io/fakeLoader.js/)
- [Wow](http://mynameismatthieu.com/WOW/)
- [Bouncejs](http://bouncejs.com/)
- [Move](https://visionmedia.github.io/move.js/)
- [Easie](http://jaukia.github.io/easie/)
- [iGrowl](http://catc.github.io/iGrowl/)
- [Morf](http://www.joelambert.co.uk/morf/)
- [Transit](http://ricostacruz.com/jquery.transit/)
- [Greensock](http://greensock.com/)

### 字体图标

- [Loveiconfonts](http://weloveiconfonts.com/)
- [Glyphter](https://glyphter.com/)
- [Perfecticons](http://perfecticons.com/)
- [Xiconeditor](http://www.xiconeditor.com/)
- [Fontello](http://fontello.com/)
- [Iconfont](http://www.iconfont.cn/)
- [Icomoon](https://icomoon.io/)
- [Font-Awesome](http://fortawesome.github.io/Font-Awesome/)
- [Glyphicons](http://glyphicons.com/)
- [Google Material Design](https://www.google.com/design/icons/)

### 资源库

- [百度静态资源共享库](http://cdn.code.baidu.com/)
- [Taobaonpm](http://npm.taobao.org/)
- [Bootcdn](http://www.bootcdn.cn/)
- [Javascripting](http://www.javascripting.com/)
- [Microjs](http://microjs.com/)
- [TodoMVC](http://todomvc.com/)


## H5 键盘自动隐藏

```js
$("input,select,textarea").blur(function () {
	setTimeout(function () {
		var scrollHeight = document.documentElement.scrollTop || document.body.scrollTop || 0;
		window.scrollTo(0, Math.max(scrollHeight - 1, 0));
	}, 100);
});
```

## js 封装php get方法

```js
var $_GET = (function () {
	var url = window.document.location.href.toString();
	var u = url.split("?");
	if (typeof (u[1]) == "string") {
		u = u[1].split("&");
		var get = {};
		for (var i in u) {
			var j = u[i].split("=");
			get[j[0]] = j[1];
		}
		return get;
	} else {
		return {};
	}
})();
```


## Flex布局新旧混合写法详解（兼容微信）

flex 是个非常好用的属性，如果说有什么可以完全代替 `float` 和 `position` ，那么肯定是非它莫属了（虽然现在还有很多不支持 flex 的浏览器）。然而国内很多浏览器对 flex 的支持都不是很好，这里针对微信内置浏览器写了一套兼容写法。下面入正题。

首先还是从两个版本的语法开始讲吧，这里还是假设 flex 容器为 `.box` ，子元素为 `.item` 。

### 旧语法篇

### 定义容器的 display 属性



```
.box{
    display: -moz-box; /*Firefox*/
    display: -webkit-box; /*Safari,Opera,Chrome*/
    display: box;
}
```

### 容器属性

#### 1. box-pack 属性

box-pack 定义子元素主轴对齐方式。

```
.box{
    -moz-box-pack: center; /*Firefox*/
    -webkit-box-pack: center; /*Safari,Opera,Chrome*/
    box-pack: center;
}
```

box-pack 属性总共有 4 个值：

```
.box{
    box-pack: start | end | center | justify;
    /*主轴对齐：左对齐（默认） | 右对齐 | 居中对齐 | 左右对齐*/
}
```

#### 2. box-align 属性

box-align定义子元素交叉轴对齐方式。

```
.box{
    -moz-box-align: center; /*Firefox*/
    -webkit-box-align: center; /*Safari,Opera,Chrome*/
    box-align: center;
}
```

box-align 属性总共有 5 个值：

```
.box{
    box-align: start | end | center | baseline | stretch;
    /*交叉轴对齐：顶部对齐（默认） | 底部对齐 | 居中对齐 | 文本基线对齐 | 上下对齐并铺满*/
}
```

#### 3. box-direction 属性

box-direction 定义子元素的显示方向。

```
.box{
    -moz-box-direction: reverse; /*Firefox*/
    -webkit-box-direction: reverse; /*Safari,Opera,Chrome*/
    box-direction: reverse;
}
```

box-direction 属性总共有 3 个值：

```
.box{
    box-direction: normal | reverse | inherit;
    /*显示方向：默认方向 | 反方向 | 继承子元素的 box-direction*/
}
```

#### 4. box-orient 属性

box-orient 定义子元素是否应水平或垂直排列。

```
.box{
    -moz-box-orient: horizontal; /*Firefox*/
    -webkit-box-orient: horizontal; /*Safari,Opera,Chrome*/
    box-orient: horizontal;
}
```

box-orient 属性总共有 5 个值：

```
.box{
    box-orient: horizontal | vertical | inline-axis | block-axis | inherit;
    /*排列方向：水平 | 垂直 | 行内方式排列（默认） | 块方式排列 | 继承父级的box-orient*/
}
```

#### 5. box-lines 属性

box-lines 定义当子元素超出了容器是否允许子元素换行。

```
.box{
    -moz-box-lines: multiple; /*Firefox*/
    -webkit-box-lines: multiple; /*Safari,Opera,Chrome*/
    box-lines: multiple;
}
```

box-lines 属性总共有 2 个值：

```
.box{
    box-lines: single | multiple;
    /*允许换行：不允许（默认） | 允许*/
}
```

### 子元素属性

#### 1. box-flex 属性

box-flex 定义是否允许当前子元素伸缩。

```
.item{
    -moz-box-flex: 1.0; /*Firefox*/
    -webkit-box-flex: 1.0; /*Safari,Opera,Chrome*/
    box-flex: 1.0;
}
```

box-flex 属性使用一个浮点值：

```
.item{
    box-flex: <value>;
    /*伸缩：<一个浮点数，默认为0.0，即表示不可伸缩，大于0的值可伸缩，柔性相对>*/
}
```

#### 2.box-ordinal-group 属性

box-ordinal-group 定义子元素的显示次序，数值越小越排前。

```
.item{
    -moz-box-ordinal-group: 1; /*Firefox*/
    -webkit-box-ordinal-group: 1; /*Safari,Opera,Chrome*/
    box-ordinal-group: 1;
}
```

box-direction 属性使用一个整数值：

```
.item{
    box-ordinal-group: <integer>;
    /*显示次序：<一个整数，默认为1，数值越小越排前>*/
}
```

## 新版语法

### 定义容器的 display 属性

```
.box{
    display: -webkit-flex; /*webkit*/
    display: flex;
}

/*行内flex*/
.box{
    display: -webkit-inline-flex; /*webkit*/
    display:inline-flex;
}
```

### 容器样式

```
.box{
    flex-direction: row | row-reverse | column | column-reverse;
    /*主轴方向：左到右（默认） | 右到左 | 上到下 | 下到上*/

    flex-wrap: nowrap | wrap | wrap-reverse;
    /*换行：不换行（默认） | 换行 | 换行并第一行在下方*/

    flex-flow: <flex-direction> || <flex-wrap>;
    /*主轴方向和换行简写*/

    justify-content: flex-start | flex-end | center | space-between | space-around;
    /*主轴对齐方式：左对齐（默认） | 右对齐 | 居中对齐 | 两端对齐 | 平均分布*/

    align-items: flex-start | flex-end | center | baseline | stretch;
    /*交叉轴对齐方式：顶部对齐（默认） | 底部对齐 | 居中对齐 | 上下对齐并铺满 | 文本基线对齐*/

    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
    /*多主轴对齐：顶部对齐（默认） | 底部对齐 | 居中对齐 | 上下对齐并铺满 | 上下平均分布*/
}
```

### 子元素属性

```
.item{
    order: <integer>;
    /*排序：数值越小，越排前，默认为0*/

    flex-grow: <number>; /* default 0 */
    /*放大：默认0（即如果有剩余空间也不放大，值为1则放大，2是1的双倍大小，以此类推）*/

    flex-shrink: <number>; /* default 1 */
    /*缩小：默认1（如果空间不足则会缩小，值为0不缩小）*/

    flex-basis: <length> | auto; /* default auto */
    /*固定大小：默认为0，可以设置px值，也可以设置百分比大小*/

    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
    /*flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto，*/

    align-self: auto | flex-start | flex-end | center | baseline | stretch;
    /*单独对齐方式：自动（默认） | 顶部对齐 | 底部对齐 | 居中对齐 | 上下对齐并铺满 | 文本基线对齐*/
}
```

## 兼容写法

首先是定义容器的 `display` 属性：

```
.box{
    display: -webkit-box; /* 老版本语法: Safari, iOS, Android browser, older WebKit browsers. */
    display: -moz-box; /* 老版本语法: Firefox (buggy) */
    display: -ms-flexbox; /* 混合版本语法: IE 10 */
    display: -webkit-flex; /* 新版本语法: Chrome 21+ */
    display: flex; /* 新版本语法: Opera 12.1, Firefox 22+ */
}
```

由于旧版语法并没有列入 W3C 标准，所以这里不用写 `display:box` ，下面的语法也是一样的。

这里还要注意的是，如果子元素是行内元素，在很多情况下都要使用 `display: block` 或 `display: inline-block` 把行内子元素变成块元素（例如使用 `box-flex` 属性），这也是旧版语法和新版语法的区别之一。

### 子元素主轴对齐方式

```
.box{
    -webkit-box-pack: center;
    -moz-justify-content: center;
    -webkit-justify-content: center;
    justify-content: center;
}
```


![](../_media/fx01.png)

这里旧版语法有 4 个参数，而新版语法有 5 个参数，兼容写法新版语法的 `space-around` 是不可用的：

```
.box{
    box-pack: start | end | center | justify;
    /*主轴对齐：左对齐（默认） | 右对齐 | 居中对齐 | 左右对齐*/

    justify-content: flex-start | flex-end | center | space-between | space-around;
    /*主轴对齐方式：左对齐（默认） | 右对齐 | 居中对齐 | 两端对齐 | 平均分布*/
}
```

### 子元素交叉轴对齐方式

```
.box{
    -webkit-box-align: center;
    -moz-align-items: center;
    -webkit-align-items: center;
    align-items: center;
}
```


![](../_media/fx03.png)

这里的参数除了写法不同，其实是功能是一样的：

```
.box{
    box-align: start | end | center | baseline | stretch;
    /*交叉轴对齐：顶部对齐（默认） | 底部对齐 | 居中对齐 | 文本基线对齐 | 上下对齐并铺满*/

    align-items: flex-start | flex-end | center | baseline | stretch;
    /*交叉轴对齐方式：顶部对齐（默认） | 底部对齐 | 居中对齐 | 上下对齐并铺满 | 文本基线对齐*/
}
```

### 子元素的显示方向

子元素的显示方向可通过 `box-direction + box-orient + flex-direction` 实现，下面请看实例：

#### 左到右

```
.box{
    -webkit-box-direction: normal;
    -webkit-box-orient: horizontal;
    -moz-flex-direction: row;
    -webkit-flex-direction: row;
    flex-direction: row;
}
```

![](../_media/fx051.png)

#### 右到左

```
.box{
    -webkit-box-pack: end;
    -webkit-box-direction: reverse;
    -webkit-box-orient: horizontal;
    -moz-flex-direction: row-reverse;
    -webkit-flex-direction: row-reverse;
    flex-direction: row-reverse;
}
```

这里补充说明一点： box 写法的 `box-direction` 只是改变了子元素的排序，并没有改变对齐方式，需要新增一个 `box-pack` 来改变对齐方式。


![](../_media/fx02.png)

#### 上到下

```
.box{
    -webkit-box-direction: normal;
    -webkit-box-orient: vertical;
    -moz-flex-direction: column;
    -webkit-flex-direction: column;
    flex-direction: column;
}
```

![](../_media/fx06.png)


#### 下到上

```
.box{
    -webkit-box-pack: end;
    -webkit-box-direction: reverse;
    -webkit-box-orient: vertical;
    -moz-flex-direction: column-reverse;
    -webkit-flex-direction: column-reverse;
    flex-direction: column-reverse;
}
```

![](../_media/fx04.png)

### 是否允许子元素伸缩

```
.item{
    -webkit-box-flex: 1.0;
    -moz-flex-grow: 1;
    -webkit-flex-grow: 1;
    flex-grow: 1;
}
```

![](../_media/fx10.png)

```
.item{
    -webkit-box-flex: 1.0;
    -moz-flex-shrink: 1;
    -webkit-flex-shrink: 1;
    flex-shrink: 1;
}
```

![](../_media/fx15.png)

上面是允许放大，box语法中 `box-flex` 如果不是 0 就表示该子元素允许伸缩，而 flex 是分开的，上面 `flex-grow` 是允许放大（默认不允许），下面的 `flex-shrink` 是允许缩小（默认允许）。`box-flex` 默认值为 0 ，也就是说，在默认的情况下，在两个浏览器中的表现是不一样的：

![](../_media/fx11.png)

这里还有一点，就是新旧语法的算法是不一样的，假设 `box-flex` 的值不等于 0 ，旧语法中，如果有多余的空间，`box-flex` 的值越大，说明空白部分的占比越多，反之亦然：

![](../_media/fx13.png)

而新版的语法中，放大的比例是直接按 `flex-grow` 的值来分配的，`flex-grow` 的缩放会覆盖 `flex-shrink: 0`，看例子：

![](../_media/fx14.png)

参数：

```
.item{
    box-flex: <value>;
    /*伸缩：<一个浮点数，默认为0.0，即表示不可伸缩，大于0的值可伸缩，柔性相对>*/

    flex-grow: <number>; /* default 0 */
    /*放大：默认0（即如果有剩余空间也不放大，值为1则放大，2是1的双倍大小，以此类推）*/

    flex-shrink: <number>; /* default 1 */
    /*缩小：默认1（如果空间不足则会缩小，值为0不缩小）*/
}
```

### 子元素的显示次序

```
.item{
    -webkit-box-ordinal-group: 1;
    -moz-order: 1;
    -webkit-order: 1;
    order: 1;
}
```

![](../_media/fx16.png)


## PHP 过滤微信昵称表情

```php
function filterNickname($nickname) {
    $nickname = preg_replace('/[\x{1F600}-\x{1F64F}]/u', '', $nickname);
    $nickname = preg_replace('/[\x{1F300}-\x{1F5FF}]/u', '', $nickname);
    $nickname = preg_replace('/[\x{1F680}-\x{1F6FF}]/u', '', $nickname);
    $nickname = preg_replace('/[\x{2600}-\x{26FF}]/u', '', $nickname);
    $nickname = preg_replace('/[\x{2700}-\x{27BF}]/u', '', $nickname);
    $nickname = str_replace(array('"', '\''), '', $nickname);
    return addslashes(trim($nickname));
}
```

## gulp 自动化

**一、什么是gulp**

gulp是一个自动化构建工具,开发者可以使用它在项目开发过程中自动执行常见任务。例如：css、js的合并与压缩（减少http请求，缩小文件大小）、html压缩、md5名生成与替换（一般解决浏览器缓存）、线上配置文件自动替换、搭建本地web服务器做到实时刷新等。

**二、环境搭建**

gulp是基于Node.js的插件工具

1. 搭建Node.js环境：<https://nodejs.org/en/download/> 下载相应系统的node安装包。进入终端：键入：node -v如果出现node版本号，则node.js安装成功。
   ![做一个合格的前端，gulp自动化构建工具入门教程](http://blog.wxall.top/uploads/allimg/190114/10335GE0-0.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)
2. 安装gulp：
   （1）、安装全局gulp：终端键入：npm install gulp -g 等待完成；
   （2）、cd进入到项目根目录 -->:终端键入：npm init 【生成node.js插件管理json文件：package.json】：如图：
   ![做一个合格的前端，gulp自动化构建工具入门教程](http://blog.wxall.top/uploads/allimg/190114/10335M0c-1.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)
   【注：如图红色箭头、一路enter即可（每一项的意义都很明确，无需解释！我只是提一下license选项，这一项是开源协议：通常的开源协议有：GPL、Apache License、 BSD、ISC.. 等等，有兴趣的可以参考：[开源协议https://baike.baidu.com/item/%E5%BC%80%E6%BA%90%E5%8D%8F%E8%AE%AE/10642383?fr=aladdin]()

（3）、键入：npm install gulp --save-dev 安装gulp到当前目录，等待安装完成。键入gulp-v 出现全局版本，和当前版本，安装完成。如图：
![做一个合格的前端，gulp自动化构建工具入门教程](http://blog.wxall.top/uploads/allimg/190114/10335K604-2.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

**键入:gulp，出现：“No gulpfile fount”，说明前三步执行成功。**

(4)、从上面提示可以看出，运行gulp需要gulpfile文件。故而：在当前文件夹新建文件：gulpfile.js备用。
![做一个合格的前端，gulp自动化构建工具入门教程](http://blog.wxall.top/uploads/allimg/190114/10335J095-3.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

**三、gulpfile.js文件编写（重点）**

在gulpfile.js中写入：

```js
var gulp = require("gulp"); //申明gulp变量
gulp.task("start",function(){
    console.log("痞子猫**************");
});
```

终端键入：gulp start 执行gulp中的start任务，出现打印，说明gulpfile.js编写没什么问题！如图：
![做一个合格的前端，gulp自动化构建工具入门教程](http://blog.wxall.top/uploads/allimg/190114/10335K158-4.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

下面开始进入正题，
压缩、合并等这些操作可以依靠gulp插件完成：插件安装命令：**npm install [module] --save-dev** ;

如：npm install gulp-concat --save-dev 这是安装合并插件。使用如下：

```js
var gulp = require("gulp"); //申明gulp变量
var concat = require('gulp-concat'); // require：加载插件，参数为：插件名

//合并任务
gulp.task("concat",function(){ //concat为任务名，运行gulp concat执行此任务。
    return gulp.src("./js/*.js")  //需要合并的js目录，支持正则
            .pipe(concat("all.js")) //concat(),合并操作，参数：合并后的名字
            .pipe(gulp.dest("./js/")) //合并后放入的目录
});
```

终端键入：gulp concat 执行此任务。

这就是一个简单的合并任务，**前面提到了node是基于流，也就是管道操作的。gulp.src（）管道的入口，pipe()获得处理管道中的数据，pipe(gulp.dest())处理后数据的出口。说别了就是一个流水线操作。**

所有的gulp插件运用也就是这个套路了：常用插件集合：
1.gulp-sass(sass编译)
2.gulp-compass(sass编译)
3.gulp-autoprefixer(添加CSS3前缀)
4.gulp-clean-css(压缩CSS)
5.gulp-include（文件包含）
6.gulp-concat(文件合并)
7.del(文件删除)
8.gulp-uglify(压缩js)
9.gulp.spritesmith(合成雪碧图)
10.run-sequence(队列执行)
11.browser-sync(浏览器同步刷新)
12.gulp-babel(js编译)
13.gulp-imagemin(图片压缩)
gulp-imageisux(腾讯智图压缩，慎用)
imagemin-jpegtran(jpg图片压缩)
imagemin-pngquant(png图片压缩)
gulp-image-resize(图片大小调整)
14.gulp-rename(重命名)
15.gulp-live-server(轻量服务器)
16.gulp-livereload
17.gulp-util(工具集)
18.require-dir(引入整个文件夹文件)
19.connect-livereload(热更新同步)
20.gulp-if(是否运行插件)
21.gulp-plumber
22.gulp-eslint(eslint代码检查)
23.gulp-htmlmin(html压缩)
24.gulp-clean(删除文件/文件夹)
25.gulp-less
26.gulp-load-plugins(加载gulp插件)

**再次强调安装插件命令：npm install [module] --save-dev**

至于运用我觉得没啥可说的：就拿常见的举个例子吧：

**del：描述：删除：**

```js
var del = require('del');
del('./dist');                      // 删除整个dist文件夹
```

**gulp-rename：描述：重命名文件。**

```js
var rename = require("gulp-rename");  

gulp.src('./hello.txt')  
  .pipe(rename('gb/goodbye.md'))    // 直接修改文件名和路径  
  .pipe(gulp.dest('./dist'));   

gulp.src('./hello.txt')  
  .pipe(rename({  
    dirname: "text",                // 路径名  
    basename: "goodbye",            // 主文件名  
    prefix: "pre-",                 // 前缀  
    suffix: "-min",                 // 后缀  
    extname: ".html"                // 扩展名  
  }))  
  .pipe(gulp.dest('./dist'));
```

**gulp-filter：描述：在虚拟文件流中过滤文件。**

```js
var filter = require('gulp-filter');  

const f = filter(['**', '!*/index.js']);  
gulp.src('js/**/*.js')  
    .pipe(f)                        // 过滤掉index.js这个文件  
    .pipe(gulp.dest('dist'));  

const f1 = filter(['**', '!*/index.js'], {restore: true});  
gulp.src('js/**/*.js')  
    .pipe(f1)                       // 过滤掉index.js这个文件  
    .pipe(uglify())                 // 对其他文件进行压缩  
    .pipe(f1.restore)               // 返回到未过滤执行的所有文件  
    .pipe(gulp.dest('dist'));       // 再对所有文件操作，包括index.js  
```

**gulp-uglify：描述：压缩js文件大小。**

```js
var uglify = require("gulp-uglify");  

gulp.src('./hello.js')  
    .pipe(uglify())                 // 直接压缩hello.js  
    .pipe(gulp.dest('./dist'))  

 gulp.src('./hello.js')  
    .pipe(uglify({  
        mangle: true,               // 是否修改变量名，默认为 true  
        compress: true,             // 是否完全压缩，默认为 true  
        preserveComments: 'all'     // 保留所有注释  
    }))  
    .pipe(gulp.dest('./dist'))  
```

**gulp-csso：描述：压缩优化css。**

```js
var csso = require('gulp-csso');  

gulp.src('./css/*.css')  
    .pipe(csso())  
    .pipe(gulp.dest('./dist/css'))  
```

**gulp-html-minify：描述：压缩HTML。**

```js
var htmlminify = require('gulp-html-minify');  

gulp.src('index.html')  
    .pipe(htmlminify())  
    .pipe(gulp.dest('./dist'))  
```

**gulp-imagemin：描述：压缩图片。**

```js
var imagemin = require('gulp-imagemin');  

gulp.src('./img/*.{jpg,png,gif,ico}')  
    .pipe(imagemin())  
    .pipe(gulp.dest('./dist/img'))  
```

**gulp-autoprefixer：描述：自动为css添加浏览器前缀。**

```js
var autoprefixer = require('gulp-autoprefixer');  

gulp.src('./css/*.css')  
    .pipe(autoprefixer())           // 直接添加前缀  
    .pipe(gulp.dest('dist'))  

gulp.src('./css/*.css')  
    .pipe(autoprefixer({  
        browsers: ['last 2 versions'],      // 浏览器版本  
        cascade：true                       // 美化属性，默认true  
        add: true                           // 是否添加前缀，默认true  
        remove: true                        // 删除过时前缀，默认true  
        flexbox: true                       // 为flexbox属性添加前缀，默认true  
    }))  
    .pipe(gulp.dest('./dist')) 
```

其实在列举没啥意思，都是一个套路，想了解那个插件的参数、使用方法等，**直接上https://www.npmjs.com/ 上面查询吧！**

注意事项：
**src入口匹配说明：**

- app.js 精确匹配
- *.js 能匹配js后缀的文件
- **/*.js 能匹配多级目录下的js文件（也包含当前目录下）
- !js/app.js 精确排除

**return说明**
不加return返回，默认异步执行；加上return意思很明确，就是等待流处理结束~可做同步处理任务条件

**默认任务说明**
只要任务名字为default，当运行gulp时是可省略任务名执行，直接键入：gulp

**任务依赖说明**

```js
gulp.task("task1",function(){
    //
});
gulp.task("task2",["task1"],function(){
    //
});
```

如上代码：task2依赖于task1，也就是说等待task1任务结束后再执行

**同步执行说明**
gulp默认全部任务为异步执行，其实在实际场景很多需要同步操作：如:拷贝-压缩操作，必须拷贝完成才能压缩。
故而推荐一种简单的方法处理同步问题：使用插件：gulp-sequence

```js
var gulp = require("gulp"); //申明gulp变量
var gulpSequence = require('gulp-sequence'); //同步执行模块
gulp.task("task1",function(){
    return xxx //具体任务
});
gulp.task("task2",function(){
     return xxx  //具体任务
});
gulp.task("task3",function(){
     return xxx  //具体任务
});
//这样可保证，task1-》task2-》task3的执行顺序，任务必须return，已解释原因
gulp.task("default",function(callback){gulpSequence("task1",
            "task2",
            "task3",
            callback);
});
```

啥？你不想看这么多！那也简单~
1、安装node 
2、执行 npm install gulp -g
3、复制下面代码到：package.json（新建）

```js
{
  "name": "guandn",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "directories": {
    "lib": "lib"
  },
  "dependencies": {
    "gulp": "^3.9.1"
  },
  "devDependencies": {
    "del": "^3.0.0",
    "gulp-concat": "^2.6.1",
    "gulp-html-minify": "0.0.14",
    "gulp-htmlmin": "^4.0.0",
    "gulp-minify-css": "^1.2.4",
    "gulp-rename": "^1.2.2",
    "gulp-rev": "^8.1.1",
    "gulp-rev-collector": "^1.2.4",
    "gulp-sequence": "^1.0.0",
    "gulp-smushit": "^1.2.0",
    "gulp-uglify": "^3.0.0",
    "gulp-useref": "^3.1.4",
    "merge-stream": "^1.0.1"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://gitee.com/quzhuanpanDesigner/guandn.git"
  },
  "author": "broom",
  "license": "ISC"
}
```

4、执行：npm install 【根据package.json安装插件】
5、复制下面代码到：gulpfile.js（新建）

```js
var gulp = require("gulp"); //申明gulp变量

var del = require('del'); //删除模块
var fs = require("fs");  //文件操作模块
var concat = require('gulp-concat'); // 拼接工具

var uglify = require('gulp-uglify'); // 压缩js工具
var minifyCss = require('gulp-minify-css'); // 压缩css工具
var miniHtml = require("gulp-html-minify"); //压缩html/jsp

var smushit = require('gulp-smushit'); //无损压缩jpg、png图片

var rev = require('gulp-rev');       // 加md5后缀的工具
var revCollector = require('gulp-rev-collector'); //与rev共同使用

var useref = require('gulp-useref'); // 替换文件中的链接的工具,一般与gulp-rev共同使用
var gulpSequence = require('gulp-sequence'); //同步执行模块
var mergeStream = require("merge-stream");  //合并多个stream

//资源配置
var config={
    dist:"./dist/guandn/",   //目标目录
    target:'./target/guandn/',  //资源
}

//MD5地图json地址
var revJsonConfig = {
    js:'./rev/rev-manifest-js.json',
    css:'./rev/rev-manifest-css.json',
    base:'./rev/'
}

//js压缩参数
var uglifyConfig = {
        mangle: false,//类型：Boolean 默认：true 是否修改变量名
        compress: true,//类型：Boolean 默认：true 是否完全压缩
    }

var Util = {
        fetchScripts: function (readFile, basePath) { //文件操作工具，取得需要合并的js
            var sources = fs.readFileSync(readFile);
            sources = /\[([^\]]+\.js'[^\]]+)\]/.exec(sources);
            sources = sources[1].replace(/\/\/.*\n/g, '\n').replace(/'|"|\n|\t|\s/g, '');
            sources = sources.split(",");
            console.log("搜索js合并地址：读取文件： "+readFile);
            sources.forEach(function (filepath, index) {
                sources[ index ] = basePath + filepath;
                console.log("地址：" + basePath + filepath);
            });

            return sources;
        },
        fetchStyles: function (readFile, basePath) {  //文件操作工具，取得需要合并的css
            var sources = fs.readFileSync(readFile),
                filepath = null,
                pattern = /@import\s+([^;]+)*;/g,
                src = [];
            console.log("搜索css合并地址：读取文件： "+readFile);
            while (filepath = pattern.exec(sources)) {
                src.push(basePath + filepath[ 1 ].replace(/'|"/g, ""));
                console.log("地址：" + basePath + filepath[ 1 ].replace(/'|"/g, ""));
            }
            return src;
        }
    };

gulp.task("copy",function(){
    console.log("拷贝代码到dist************************")
    return gulp.src(config.target+"/**/*").
                pipe(gulp.dest(config.dist));
});
gulp.task("del",function(){
    console.log("删除dist、rev目录************************")
    return del([config.dist,revJsonConfig.base]);
});

//合并任务
gulp.task("combine",function(){

    console.log("合并任务开始************************")
    var commonJs = Util.fetchScripts(config.dist+"media/common.app.js", config.dist+"media/");
    var commJsStream =  gulp.src(commonJs)
        .pipe(concat("common.app.js"))
        .pipe(uglify()) //压缩
        .pipe(rev())    //加名字MD5
        .pipe(gulp.dest(config.dist+"media/")) //保存
        .pipe(rev.manifest({ //生成MD5地图
            path: revJsonConfig.js,
            merge: true
        })) 
        .pipe(gulp.dest("./"));

    var commonCss = Util.fetchStyles(config.dist + "media/common.app.css", config.dist + "media/");
    var commCssStream = gulp.src(commonCss)
        .pipe(concat("common.app.css"))
        .pipe(minifyCss())
        .pipe(rev())
        .pipe(gulp.dest(config.dist+"media/"))
        .pipe(rev.manifest({
            path: revJsonConfig.css,
            merge: true
        })) 
        .pipe(gulp.dest("./"));

    var pluginsJs = Util.fetchScripts(config.dist+"media/plugins/plugin.common.app.js", config.dist+"media/plugins/");
    var pluginJsStream = gulp.src(pluginsJs)
        .pipe(concat("plugin.common.app.js"))
        .pipe(uglify())
        //.pipe($.rev())
        .pipe(gulp.dest(config.dist+"media/plugins/"))
        //.pipe($.rev.manifest({
        //  path: revJsonConfig.js,
        //  merge: true
        //})) 
        //.pipe(gulp.dest("./"));

    var pluginCss = Util.fetchStyles(config.dist + "media/plugins/plugin.common.app.css", config.dist+"media/plugins/");
    var pluginCssStream = gulp.src(pluginCss)
        .pipe(concat("plugin.common.app.css"))
        .pipe(minifyCss())
        //.pipe($.rev())
        .pipe(gulp.dest(config.dist+"media/plugins/"))
        //.pipe($.rev.manifest({
        //  path: revJsonConfig.css,
        //  merge: true
        //})) 
        //.pipe(gulp.dest("./"));

    //合并编辑器
    var guandnEditor = Util.fetchScripts(config.dist+"media/plugins/guandnEditor/editor.commbine.min.js", config.dist+"media/plugins/");
    var editor = gulp.src(guandnEditor)
        .pipe(concat("editor.commbine.min.js"))
        .pipe(uglify())
        .pipe(rev())
        .pipe(gulp.dest(config.dist+"media/plugins/guandnEditor/"))
        .pipe(rev.manifest({
            path: revJsonConfig.js,
            merge: true
        })) 
        .pipe(gulp.dest("./"));

    //合并上传插件
    var guanzhi = Util.fetchScripts(config.dist+"media/plugins/jQuery-File-Upload-master/js/jquery.fileuplaod.all.min.js", config.dist+"media/plugins/")
    var guanzhiFileUulpad = gulp.src(guanzhi)
            .pipe(concat("jquery.fileuplaod.all.min.js"))
            .pipe(uglify(uglifyConfig))
            .pipe(gulp.dest(config.dist+"media/plugins/jQuery-File-Upload-master/js/"))
    return mergeStream(commJsStream,commCssStream,pluginJsStream,pluginCssStream,editor,guanzhiFileUulpad);
});

//压缩js，css
gulp.task("compress",function(callback){
    console.log("压缩任务开始***************************")
    var compressCss = gulp.src([config.dist+"media/**/*.css","!**/*.min.css","!**/plugins/**/*"])
        .pipe(minifyCss())
        .pipe(gulp.dest(config.dist+"media/")); 
    var compressJs = gulp.src([config.dist+"media/**/*.js","!**/*.min.js","!**/plugins/**/*"])
        .pipe(uglify(uglifyConfig))
        .pipe(gulp.dest(config.dist+"media/"));

    return mergeStream(compressCss, compressJs);
});

//静态文件替换
gulp.task("replaceJC", function(){
    console.log("替换静态文件**************************");
    var revJson = revJsonConfig.base+"**/*.json";
    var commonCss = gulp.src([revJson, config.dist+"media/css/common/*.jsp"])  
        .pipe(revCollector({  
            replaceReved: true 
        }))
        .pipe(gulp.dest(config.dist+"media/css/common/"));

    var commonJs = gulp.src([revJson, config.dist+"media/js/common/*.jsp"])  
        .pipe(revCollector({  
            replaceReved: true 
        }))
        .pipe(gulp.dest(config.dist+"media/js/common/"));

    var editor = gulp.src([revJson, config.dist+"WEB-INF/pages/**/*.jsp"])  
    .pipe(revCollector({  
        replaceReved: true 
    }))
    .pipe(gulp.dest(config.dist+"WEB-INF/pages/")); 

    return mergeStream(commonCss, commonJs);
});

//可异步执行操作【最后操作】
gulp.task("asyn", function(){
    console.log("压缩jsp、拷贝配置**********************************")
//  gulp.src(config.dist+"WEB-INF/pages/**/*.jsp")
//      .pipe(miniHtml({
//          empty: true  //删除空格
//      }))
//      .pipe(gulp.dest(config.dist + "WEB-INF/pages/"));

    //图片压缩（费时操作）
//  gulp.src(config.dist+'media/img/**/*')
//      .pipe(smushit({
//          verbose: true
//      }))
//      .pipe(gulp.dest(config.dist+'media/img/'));
});

gulp.task("finishedDel",function(){
    return del([revJsonConfig.base]);
});

gulp.task("default",function(callback){
    gulpSequence("del",
            "copy",
            "combine",
            "compress",
            "replaceJC",
            "finishedDel",
            "asyn",
            callback);
});
```

6、修改gulpfile中的路径，文件等.

至此gulp的基本使用已经没啥问题了！

## less 入门

官网地址：<http://lesscss.cn/>
我们都知道写 `CSS` 代码是有些枯燥无味的，尤其是面对那些成千上万行 `CSS` 代码的项目。你始终在相同的地方使用相同的规则并且在你的编译器中搜索和替换每次颜色的变化。这需要很多的努力和规则来保持你的 `CSS` 可维护，但它本不应该这样的。

很幸运，网站开发社区已经解决了这个问题，现在我们拥有诸如 [Less](https://link.jianshu.com/?t=http://lesscss.org/), [Sass](https://link.jianshu.com/?t=http://sass-lang.com/) 和 [Stylus](https://link.jianshu.com/?t=http://learnboost.github.io/stylus/) 之类的预处理器，它们给我们提供了许多优于纯 `CSS` 的好处。

- 变量 - 它可以让你更轻松的在整个样式表中定义和更改值（这个功能 `CSS` 在未来某一天也有可能会实现）。
- 动态计算值 - `CSS` 中最近出了一个 `cal()`， 但它只适合用于长度的计算。
- Mixins - 可以让你重用或者组合样式，而且支持传递参数。
- 函数 - 它为你提供了一些方便的程序去操纵颜色，转换图像等。

使用预处理器的唯一缺点就是，你需要将代码转换为纯 `CSS` 代码，让它能够在浏览器中工作。

------

### 1. Getting Started

`Less` 是用 `JavaScript` 写的，所以需要额外的 `Node.js` 或者网页浏览器才能够运行它。你可以在你的网站中引入 `less.js` ，这样在真正的运行环境下它就可以自动编译，但这个过程非常缓慢，所以不建议这么使用。 推荐的方式是提前编译成 `CSS` 代码并且将一个正常的发展版本备份在线上。当然还有很多可视化的的程序帮助你编译 `less` 文件，但是在本篇文章中我们将使用 `node.js`。

如果你已经安装了 `Node` ， 那么你应该知道什么是终端，接下来就打开一个终端。安装 `less` 用以下语句 ：

```
npm install -g less
```

安装完成后，你将可以在任何打开的窗口中使用 `lessc` 命令，这个命令允许你将 `.less` 文件编译成纯 `CSS` 文件，像下面这样：

```
lessc styles.less > styles.css
```

如果说，我们用 `less` 将所有的样式写在了 `style.less` 中，通过上述命令，我们就可以将代码转换为纯 `CSS` 代码。接下来你就可以将样式表引入到 `HTML` 中了，如果在编译过程中出现了错误，将会在终端的命令行中提示你。

------

### 2. 变量

`Less` 的一个主要功能就是可以让你像在其它高级语言中一样声明变量，这样你就可以存储你经常使用的任何类型的值 ： 颜色，尺寸，选择器，字体名称和 `URL` 等。`less` 的哲学是在可能的情况下重用CSS语法。

这里，我们声明了两个变量，一个是背景颜色，一个是文本颜色，它们都是十六进制的值。

`Less` 代码如下：

```
@background-color: #ffffff;
@text-color: #1A237E;

p{
  background-color: @background-color;
  color: @text-color;
  padding: 15px;
}

ul{
  background-color: @background-color;
}

li{
  color: @text-color;
}
```

将其编译成 `CSS` 代码如下：

```
p{
    background-color: #ffffff;
    color: #1A237E;
    padding: 15px;
}

ul{
    background-color: #ffffff;
}

li{
    color: #1A237E;
}
```

在上面的例子当中，背景颜色是白色，文本颜色是黑色。比方说，现在我们要切换二者的值，也就是黑色的背景和白色的文本，我们只需要修改两个变量的值就可以了，而不是手动的去修改每个值。

阅读更多有关 `Less` 变量的内容，[请看这里](https://link.jianshu.com/?t=http://lesscss.org/features/#variables-feature)。

------

### 3. Mixins

`Less` 允许我们将已有的 `class` 和 `id` 的样式应用到另一个不同的选择器上。 下面这个例子可以清楚地说明这一点。

```
#circle{
  background-color: #4CAF50;
  border-radius: 100%;
}

#small-circle{
  width: 50px;
  height: 50px;
  #circle
}

#big-circle{
  width: 100px;
  height: 100px;
  #circle
}
```

将其转换成 `CSS` 代码如下

```
#circle {
    background-color: #4CAF50;
    border-radius: 100%;
}
#small-circle {
    width: 50px;
    height: 50px;
    background-color: #4CAF50;
    border-radius: 100%;
}
#big-circle {
    width: 100px;
    height: 100px;
    background-color: #4CAF50;
    border-radius: 100%;
}
```

如果你不想 mixin 也以一种规则的形式出现在 `CSS` 代码中，那么你可以在它的后面加上括号：

```
#circle(){
    background-color: #4CAF50;
    border-radius: 100%;
}

#small-circle{
    width: 50px;
    height: 50px;
    #circle
}

#big-circle{
    width: 100px;
    height: 100px;
    #circle
}
```

此时编译成 `CSS` :

```
#small-circle {
    width: 50px;
    height: 50px;
    background-color: #4CAF50;
    border-radius: 100%;
}
#big-circle {
    width: 100px;
    height: 100px;
    background-color: #4CAF50;
    border-radius: 100%;
}
```

`Mixin` 另一个比较酷的功能就是它支持传入参数，下面这个例子就为 `circle` 传入一个指定宽高的参数，默认是 25px。 这将创建一个 25×25的小圆和一个 100×100 的大圆。

```
#circle(@size: 25px){
    background-color: #4CAF50;
    border-radius: 100%;

    width: @size;
    height: @size;
}

#small-circle{
    #circle
}

#big-circle{
    #circle(100px)
}
```

转换成 `CSS` :

```
#small-circle {
    background-color: #4CAF50;
    border-radius: 100%;
    width: 25px;
    height: 25px;
}
#big-circle {
    background-color: #4CAF50;
    border-radius: 100%;
    width: 100px;
    height: 100px;
}
```

在 [官方文档](https://link.jianshu.com/?t=http://lesscss.org/features/#mixins-feature) 了解更多关于 `mixin` 的知识。

------

### 4. 嵌套

嵌套可用于以与页面的HTML结构相匹配的方式构造样式表，同时减少了冲突的机会。下面是一个无序列表的例子。

```
ul{
    background-color: #03A9F4;
    padding: 10px;
    list-style: none;

    li{
        background-color: #fff;
        border-radius: 3px;
        margin: 10px 0;
    }
}
```

编译成 `CSS` 代码：

```
ul {
    background-color: #03A9F4;
    padding: 10px;
    list-style: none;
}
ul li {
    background-color: #fff;
    border-radius: 3px;
    margin: 10px 0;
}
```

就像在其它高级语言中一样， `Less` 的变量根据范围接受它们的值。如果在指定范围内没有关于变量值的声明， `less` 会一直往上查找，直至找到离它最近的声明。

回到 `CSS` 中来，我们的 `li` 标签将有白色的文本，如果我们在 `ul` 标签中声明 `@text-color` 规则。

```
@text-color: #000000;

ul{
    @text-color: #fff;
    background-color: #03A9F4;
    padding: 10px;
    list-style: none;

    li{
        color: @text-color;
        border-radius: 3px;
        margin: 10px 0;
    }
}
```

编译生成的 `CSS` 代码如下：

```
ul {
    background-color: #03A9F4;
    padding: 10px;
    list-style: none;
}
ul li {
    color: #ffffff;
    border-radius: 3px;
    margin: 10px 0;
}
```

在 [这里](https://link.jianshu.com/?t=http://lesscss.org/features/#features-overview-feature-scope) 了解更多关于作用域的知识。

------

### 5. 运算

你可以对数值和颜色进行基本的数学运算。比如说我们想要两个紧邻的 `div` 标签，第二个标签是第一个标签的两倍宽并且拥有不同的背景色。

```
@div-width: 100px;
@color: #03A9F4;

div{
    height: 50px;
    display: inline-block;
}

#left{
    width: @div-width;
    background-color: @color - 100;
}

#right{
    width: @div-width * 2;
    background-color: @color;
}
```

编译成 `CSS` 如下：

```
div {
    height: 50px;
    display: inline-block;
}
#left {
    width: 100px;
    background-color: #004590;
}
#right {
    width: 200px;
    background-color: #03a9f4;
}
```

------

### 6. 函数

`Less` 中也有函数，这让它看起来像一门编程语言了，不是吗？

让我们来看一下 `fadeout`， 一个降低颜色透明度的函数。

```
@var: #004590;

div{
  height: 50px;
  width: 50px;
  background-color: @var;

  &:hover{
    background-color: fadeout(@var, 50%)
  }
}
```

编译成 `CSS` 如下所示：

```
div {
    height: 50px;
    width: 50px;
    background-color: #004590;
}
div:hover {
    background-color: rgba(0, 69, 144, 0.5);
}
```

通过上述代码，当我们将鼠标悬浮在 `div` 上时，就可以获取半透明度的动画效果，这比之前自己手动设置要简单的多了。还有很多有用的函数去操纵颜色，检测图像的大小，甚至将资源作为data-uri嵌入样式表，在 [这里](https://link.jianshu.com/?t=http://lesscss.org/functions/) 查看这些函数的列表。


## Markdown简明语法手册

标签： Cmd-Markdown

---

### 1. 斜体和粗体

使用 * 和 ** 表示斜体和粗体。

示例：

这是 *斜体*，这是 **粗体**。

### 2. 分级标题

使用 === 表示一级标题，使用 --- 表示二级标题。

示例：

```
这是一个一级标题
============================

这是一个二级标题
--------------------------------------------------

### 这是一个三级标题
```

你也可以选择在行首加井号表示不同级别的标题 (H1-H6)，例如：# H1, ## H2, ### H3，#### H4。

### 3. 外链接

使用 \[描述](链接地址) 为文字增加外链接。

示例：

这是去往 [本人博客](https://aweixin.github.io/) 的链接。

### 4. 无序列表

使用 *，+，- 表示无序列表。

示例：

- 无序列表项 一
- 无序列表项 二
- 无序列表项 三

### 5. 有序列表

使用数字和点表示有序列表。

示例：

1. 有序列表项 一
2. 有序列表项 二
3. 有序列表项 三

### 6. 文字引用

使用 > 表示文字引用。

示例：

> 野火烧不尽，春风吹又生。

### 7. 行内代码块

使用 \`代码` 表示行内代码块。

示例：

让我们聊聊 `html`。

### 8.  代码块

使用 四个缩进空格 表示代码块。

示例：

    这是一个代码块，此行左侧有四个不可见的空格。

### 9.  插入图像

使用 \!\[描述](图片链接地址) 插入图像。

示例：

![我的头像](https://aweixin.github.io/_media/weixin.jpg)




### 1. 内容目录

在段落中填写 `[TOC]` 以显示全文内容的目录结构。

[TOC]

### 2. 标签分类

在编辑区任意行的列首位置输入以下代码给文稿标签：

标签： 数学 英语 Markdown

或者

Tags： 数学 英语 Markdown

### 3. 删除线

使用 ~~ 表示删除线。

~~这是一段错误的文本。~~

### 4. 注脚

使用 [^keyword] 表示注脚。

这是一个注脚[^footnote]的样例。

这是第二个注脚[^footnote2]的样例。

### 5. LaTeX 公式

$ 表示行内公式： 

质能守恒方程可以用一个很简洁的方程式 $E=mc^2$ 来表达。

$$ 表示整行公式：

$$\sum_{i=1}^n a_i=0$$

$$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2 $$

$$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$

访问 [MathJax](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference) 参考更多使用方法。

### 6. 加强的代码块

支持四十一种编程语言的语法高亮的显示，行号显示。

非代码示例：

```
$ sudo apt-get install vim-gnome
```

Python 示例：

```python
@requires_authorization
def somefunc(param1='', param2=0):
    '''A docstring'''
    if param1 > param2: # interesting
        print 'Greater'
    return (param2 - param1 + 1) or None

class SomeClass:
    pass

>>> message = '''interpreter
... prompt'''
```

JavaScript 示例：

``` javascript
/**
* nth element in the fibonacci series.
* @param n >= 0
* @return the nth element, >= 0.
*/
function fib(n) {
  var a = 1, b = 1;
  var tmp;
  while (--n >= 0) {
    tmp = a;
    a += b;
    b = tmp;
  }
  return a;
}

document.write(fib(10));
```

### 7. 流程图

> 示例

```flow
st=>start: Start:>https://www.zybuluo.com
io=>inputoutput: verification
op=>operation: Your Operation
cond=>condition: Yes or No?
sub=>subroutine: Your Subroutine
e=>end

st->io->op->cond
cond(yes)->e
cond(no)->sub->io
```

> 更多语法参考：[流程图语法参考](http://adrai.github.io/flowchart.js/)

### 8. 序列图

#### 示例 1

```seq
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```

#### 示例 2

```seq
Title: Here is a title
A->B: Normal line
B-->C: Dashed line
C->>D: Open arrow
D-->>A: Dashed open arrow
```

> 更多语法参考：[序列图语法参考](http://bramp.github.io/js-sequence-diagrams/)

### 9. 甘特图

甘特图内在思想简单。基本是一条线条图，横轴表示时间，纵轴表示活动（项目），线条表示在整个期间上计划和实际的活动完成情况。它直观地表明任务计划在什么时候进行，及实际进展与计划要求的对比。

```gantt
    title 项目开发流程
    section 项目确定
        需求分析       :a1, 2016-06-22, 3d
        可行性报告     :after a1, 5d
        概念验证       : 5d
    section 项目实施
        概要设计      :2016-07-05  , 5d
        详细设计      :2016-07-08, 10d
        编码          :2016-07-15, 10d
        测试          :2016-07-22, 5d
    section 发布验收
        发布: 2d
        验收: 3d
```

> 更多语法参考：[甘特图语法参考](https://knsv.github.io/mermaid/#gant-diagrams)

### 10. Mermaid 流程图

```graphLR
    A[Hard edge] -->|Link text| B(Round edge)
    B --> C{Decision}
    C -->|One| D[Result one]
    C -->|Two| E[Result two]
```

### 更多语法参考：[Mermaid 流程图语法参考](https://knsv.github.io/mermaid/#flowcharts-basic-syntax)

### 11. Mermaid 序列图

```sequence
    Alice->John: Hello John, how are you?
    loop every minute
        John-->Alice: Great!
    end
```
> 更多语法参考：[Mermaid 序列图语法参考](https://knsv.github.io/mermaid/#sequence-diagrams)

### 12. 表格支持

| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |


### 13. 定义型列表

名词 1
:   定义 1（左侧有一个可见的冒号和四个不可见的空格）

代码块 2
:   这是代码块的定义（左侧有一个可见的冒号和四个不可见的空格）

        代码块（左侧有八个不可见的空格）

### 14. Html 标签

本站支持在 Markdown 语法中嵌套 Html 标签，譬如，你可以用 Html 写一个纵跨两行的表格：

    <table>
        <tr>
            <th rowspan="2">值班人员</th>
            <th>星期一</th>
            <th>星期二</th>
            <th>星期三</th>
        </tr>
        <tr>
            <td>李强</td>
            <td>张明</td>
            <td>王平</td>
        </tr>
    </table>


<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>

### 15. 内嵌图标

本站的图标系统对外开放，在文档中输入

    <i class="icon-weibo"></i>

即显示微博的图标： <i class="icon-weibo icon-2x"></i>

替换 上述 `i 标签` 内的 `icon-weibo` 以显示不同的图标，例如：

    <i class="icon-renren"></i>

即显示人人的图标： <i class="icon-renren icon-2x"></i>

更多的图标和玩法可以参看 [font-awesome](http://fortawesome.github.io/Font-Awesome/3.2.1/icons/) 官方网站。

### 16. 待办事宜 Todo 列表

使用带有 [ ] 或 [x] （未完成或已完成）项的列表语法撰写一个待办事宜列表，并且支持子列表嵌套以及混用Markdown语法，例如：

    - [ ] **Cmd Markdown 开发**
        - [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
        - [ ] 支持以 PDF 格式导出文稿
        - [x] 新增Todo列表功能 [语法参考](https://github.com/blog/1375-task-lists-in-gfm-issues-pulls-comments)
        - [x] 改进 LaTex 功能
            - [x] 修复 LaTex 公式渲染问题
            - [x] 新增 LaTex 公式编号功能 [语法参考](http://docs.mathjax.org/en/latest/tex.html#tex-eq-numbers)
    - [ ] **七月旅行准备**
        - [ ] 准备邮轮上需要携带的物品
        - [ ] 浏览日本免税店的物品
        - [x] 购买蓝宝石公主号七月一日的船票

对应显示如下待办事宜 Todo 列表：
        
- [ ] **Cmd Markdown 开发**
    - [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
    - [ ] 支持以 PDF 格式导出文稿
    - [x] 新增Todo列表功能 [语法参考](https://github.com/blog/1375-task-lists-in-gfm-issues-pulls-comments)
    - [x] 改进 LaTex 功能
        - [x] 修复 LaTex 公式渲染问题
        - [x] 新增 LaTex 公式编号功能 [语法参考](http://docs.mathjax.org/en/latest/tex.html#tex-eq-numbers)
- [ ] **七月旅行准备**
    - [ ] 准备邮轮上需要携带的物品
    - [ ] 浏览日本免税店的物品
    - [x] 购买蓝宝石公主号七月一日的船票
      
        
[^footnote]: 这是一个 *注脚* 的 **文本**。

[^footnote2]: 这是另一个 *注脚* 的 **文本**。



## css-reset

```css
body {
    font-family: "Helvetica Neue", Helvetica, STHeiTi, sans-serif; /*使用无衬线字体*/
}

a, img {
    -webkit-touch-callout: none; /*禁止长按链接与图片弹出菜单*/
}

html, body {
    -webkit-user-select: none; /*禁止选中文本*/
    user-select: none;
}

button,input,optgroup,select,textarea {
    -webkit-appearance:none; /*去掉webkit默认的表单样式*/
}

a,button,input,optgroup,select,textarea {
    -webkit-tap-highlight-color:rgba(0,0,0,0); /*去掉a、input和button点击时的蓝色外边框和灰色半透明背景*/
}

input::-webkit-input-placeholder {
    color:#ccc; /*修改webkit中input的planceholder样式*/
}

input:focus::-webkit-input-placeholder {
    color:#f00; /*修改webkit中focus状态下input的planceholder样式*/
}

body {
    -webkit-text-size-adjust: 100%!important; /*禁止IOS调整字体大小*/
}

input::-webkit-input-speech-button {
    display: none; /*隐藏Android的语音输入按钮*/
}
```



## Git常用命令速查手册


> **Git的四个组成部分**


1、初始化仓库

```js
git init
```

2、将文件添加到仓库

```js
git add 文件名 # 将工作区的某个文件添加到暂存区   
git add -u # 添加所有被tracked文件中被修改或删除的文件信息到暂存区，不处理untracked的文件
git add -A # 添加所有被tracked文件中被修改或删除的文件信息到暂存区，包括untracked的文件
git add . # 将当前工作区的所有文件都加入暂存区
git add -i # 进入交互界面模式，按需添加文件到缓存区
```

3、将暂存区文件提交到本地仓库

```js
git commit -m "提交说明" # 将暂存区内容提交到本地仓库
git commit -a -m "提交说明" # 跳过缓存区操作，直接把工作区内容提交到本地仓库
```

4、查看仓库当前状态

```js
git status
```

5、比较文件异同

```js
git diff # 工作区与暂存区的差异
git diff 分支名 #工作区与某分支的差异，远程分支这样写：remotes/origin/分支名
git diff HEAD  # 工作区与HEAD指针指向的内容差异
git diff 提交id 文件路径 # 工作区某文件当前版本与历史版本的差异
git diff --stage # 工作区文件与上次提交的差异(1.6 版本前用 --cached)
git diff 版本TAG # 查看从某个版本后都改动内容
git diff 分支A 分支B # 比较从分支A和分支B的差异(也支持比较两个TAG)
git diff 分支A...分支B # 比较两分支在分开后各自的改动

# 另外：如果只想统计哪些文件被改动，多少行被改动，可以添加 --stat 参数
```

6、查看历史记录

```js
git log # 查看所有commit记录(SHA-A校验和，作者名称，邮箱，提交时间，提交说明)
git log -p -次数 # 查看最近多少次的提交记录
git log --stat # 简略显示每次提交的内容更改
git log --name-only # 仅显示已修改的文件清单
git log --name-status # 显示新增，修改，删除的文件清单
git log --oneline # 让提交记录以精简的一行输出
git log –graph –all --online # 图形展示分支的合并历史
git log --author=作者  # 查询作者的提交记录(和grep同时使用要加一个--all--match参数)
git log --grep=过滤信息 # 列出提交信息中包含过滤信息的提交记录
git log -S查询内容 # 和--grep类似，S和查询内容间没有空格
git log fileName # 查看某文件的修改记录，找背锅专用
```

7、代码回滚

```js
git reset HEAD^ # 恢复成上次提交的版本
git reset HEAD^^ # 恢复成上上次提交的版本，就是多个^，以此类推或用~次数

git reflog

git reset --hard 版本号

--soft：只是改变HEAD指针指向，缓存区和工作区不变；
--mixed：修改HEAD指针指向，暂存区内容丢失，工作区不变；
--hard：修改HEAD指针指向，暂存区内容丢失，工作区恢复以前状态；
```

8、同步远程仓库

```js
git push -u origin master
```

9、删除版本库文件

```js
git rm 文件名
```

10、版本库里的版本替换工作区的版本

```js
git checkout -- test.txt
```

11、本地仓库内容推送到远程仓库

```js
git remote add origin git@github.com:帐号名/仓库名.git
```

12、从远程仓库克隆项目到本地

```js
git clone git@github.com:git帐号名/仓库名.git
```

13、创建分支

```js
git checkout -b dev
-b表示创建并切换分支
上面一条命令相当于一面的二条：
git branch dev //创建分支
git checkout dev //切换分支
```

14、查看分支

```js
git branch
```

15、合并分支

```js
git merge dev
//用于合并指定分支到当前分支

git merge --no-ff -m "merge with no-ff" dev
//加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并
```

16、删除分支

```
git branch -d dev
```

17、查看分支合并图

```js
git log --graph --pretty=oneline --abbrev-commit
```

18、查看远程库信息

```js
git remote
// -v 显示更详细的信息
```

19、git相关配置

```js
# 安装完Git后第一件要做的事，设置用户信息(global可换成local在单独项目生效)：
git config --global user.name "用户名" # 设置用户名
git config --global user.email "用户邮箱"   #设置邮箱
git config --global user.name   # 查看用户名是否配置成功
git config --global user.email   # 查看邮箱是否配置

# 其他查看配置相关
git config --global --list  # 查看全局设置相关参数列表
git config --local --list # 查看本地设置相关参数列表
git config --system --list # 查看系统配置参数列表
git config --list  # 查看所有Git的配置(全局+本地+系统)
git config --global color.ui true //显示git相关颜色
```

20、撤消某次提交

```js
git revert HEAD # 撤销最近的一个提交
git revert 版本号 # 撤销某次commit
```

21、拉取远程分支到本地仓库

```js
git checkout -b 本地分支 远程分支 # 会在本地新建分支，并自动切换到该分支
git fetch origin 远程分支:本地分支 # 会在本地新建分支，但不会自动切换，还需checkout
git branch --set-upstream 本地分支 远程分支 # 建立本地分支与远程分支的链接
```

22、标签命令

```js
git tag 标签 //打标签命令，默认为HEAD
git tag //显示所有标签
git tag 标签 �版本号 //给某个commit版本添加标签
git show 标签 //显示某个标签的详细信息
```

23、同步远程仓库更新

```js
git fetch  origin master
 //从远程获取最新的到本地，首先从远程的origin的master主分支下载最新的版本到origin/master分支上，然后比较本地的master分支和origin/master分支的差别，最后进行合并。

git fetch比git pull更加安全
```


## npm 更新和noide.js 更新

更新你已经安装的NPM库，这个很简单，只需要运行。

```
npm update -g
```

更新Nodejs自身。一直依赖我都是下载最新版的源码，然后make install，及其繁琐。其实只需要运行以下2个命令即可：

```
npm install -g n 
n latest
```


## 内网穿透—外网可以访问到本地页面
今天在解决跨域问的时候，误打误撞解决了一个原来困扰我的问题。

很多时候会有这样的问题，想要把本地的页面发送给别人看，不可能一个demo页面都去部署到线上去，那该怎么办？

如果是内网问题还好解决，如果用的是webstorm的话，webstorm本身就可以自己在本地起一个服务。在webstorm的偏好设置里面，有一个`debugger`选项，只要把`Built-in server`这个选项勾选，webstorm会自己启动一个服务，只要把网址的`localhost`改成自己机器的ip地址，只要机器是在同一个网内，就可以访问到本地的页面了。

还有一种方式是，利用node在本地起一个服务，方法是安装一个npm包`http-server`，运行`npm install http-server -g`，安装完成之后直接输入命令`$ http-server`，他就会在本地的8080端口去起一个服务，本地所有的文件就都可以访问到了，只要在起的服务的地址后面再加上文件的路径就可以了。

如果是外网呢？我在知乎上看到了一个答案，([这里](https://www.zhihu.com/question/25456655))。

具体做法是这样的，我们需要一个工具`ngrok`，但是好像这个已经被墙了，国内有很多替代的工具，可以自行搜索一下，这里我找到两个：

1. [natapp.cn](http://natapp.cn/)
2. [Sunny-Ngrok](http://www.ngrok.cc/)

具体的使用方法也很简单，以natapp来举例。只要下载相应操作系统的客户端，然后把它解压，cd进入到解压后的目录中，运行命令`./ngrok -config ngrok.cfg -subdomain myapp 80`。

**这里需要注意myapp这个是你要自己定义的域名，80是你要映射到本地的端口，然后访问http://myapp.ngrok.natapp.cn就可以和本地联通了。**

现在其实只要配合node在本地起的服务就可以完成外网访问本地页面了，使用`http-server`起一个本地的服务是8080端口，所以我只需要把`ngrok`映射到8080端口就可以了，即运行指令`./ngrok -config ngrok.cfg -subdomain myapp 8080`，这样再访问自定义的域名之后，就可以顺利访问到本地了。

最后，很多选项都可以在`ngrok.cfg`这个文件里修改，并且`ngrok`的用途不仅仅简单地是这一个，他还有很多用途，有兴趣的具体可以去网上了解一下，因为我对`ngrok`的了解也不是很多。

在解决这个问题之后，我本地的页面已经可以跑在微信里了，再配合我原来记录的[手机调试本地页面的跨域问题](http://www.yatessss.com/2016/01/26/Charles解决跨域问题/)，就可以解决接口跨域的问题了，不过这个解决办法挺笨的，更好的解决办法我还在找，但是还没有找到。

最好还是和后台沟通可以把测试服务器的接口改成所有域名都能够访问，这样还是最方便的。

以上。




  ## Dos—Lsp修复

打开电脑，进入命令提示符窗口，快捷键win+r.
在窗口中输入“cmd”进入命令符窗口。
在窗口中输入：输入```netsh winsock reset```，然后按下回车键。
然后稍等片刻，出现提示，重启电脑即可。
