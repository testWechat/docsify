微信小程序只支持原生css写法，但是前端开发less和sass已经很普及。

网上有很多webstrom编译less的方法，个人觉得比较麻烦。

下面介绍一个less编译的方法：

1、npm或者yarn全局安装wxss-cli

```javascript
npm install -g wxss-cli
```

2、运行wxss-cli命令(miniProject为小程序目录)，less文件保存时自动编译

```javascript
wxss ./miniProject
```

