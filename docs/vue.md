## vue打包项目
> 这次给大家带来Vue在打包项目以后刷新显示404应该怎么处理，处理Vue在打包项目以后刷新显示404的注意事项有哪些。

```base
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



## 显示变量的解决方法

```js
    [v-cloak] {
        display: none !important;
    }
    v-cloak
```

## Vue本地部署

> history 模式下 页面空白不显示
> 修改config 下 `index.js` assetsPublicPath 为:'./'

```js
  mode: 'history',
  base: '/2020/7/vuetest', // 需要指定当前网站更目录
```
