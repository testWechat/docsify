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