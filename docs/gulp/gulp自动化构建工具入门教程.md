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