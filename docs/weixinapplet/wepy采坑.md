wepy是一个微信小程序框架，支持模块化开发，开发风格类似Vue.js。可搭配redux使用，能同时打包出web和小程序。

官方文档 <https://tencent.github.io/wepy>

## 目录结构：

![img](http://upload-images.jianshu.io/upload_images/8553499-0b07271886b24401.png)

`pages`: 存放主页面
`sotre`: redux(如果你创建项目时使用了`redux`的话)
`components`: 存放组件
`mixins`: 混合组件
`wepy.config.js`: `webpack`配置文件
`app.wpy`: 小程序入口文件
`project.config.json`: 小程序项目配置文件
`index.template.html`: web页面的入口文件

## mixins:

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

## methods, events:

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

## 关于computed:

`wepy`中也有`computed`,`props`，`data`,`watch`等`vue`中有的一些属性（没有`filter`, `directive`）。
`props`,`data`,`watch`和`vue`基本无异。

`wepy`中`computed`计算属性是无法传参的（本人没能找到传参的方法，且官方文档没有提到），在处理一些动态数据的时候，只能通过其他方法来操作。
比如，服务端获取到的的JSON对象内有条时间戳数据需要转换成字符串，我的做法是将时间戳另外传值给子组件，然后在子组件中使用`computed`对`props`进行记算。

## 事件传值：

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

## 组件传值：

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

## 动态绑定class:

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

## 循环渲染组件：

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

# wx:if

wepy中使用 `wx:if`，只阻止视图渲染，不会阻止组件初始化。
如在子组件onLoad 生命周期或者计算属性中使用了一些父级传递过来的动态数据，就会报错。