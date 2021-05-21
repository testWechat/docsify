## 更新NPM

>更新你已经安装的NPM库，这个很简单，只需要运行。

```bash
npm update -g
```
## 更新Nodejs

```bash
npm install -g n 
n latest
```
## npm 安装包报错

```js
原因事设置的代理错误，删除即可

npm config rm proxy
npm config rm https-proxy

然后使用npm install -g cnpm --registry=https://registry.npm.taobao.org
安装淘宝的cnpm
然后就可以使用cnpm命令了

```

