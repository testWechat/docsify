## reset

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
## 常用公用CSS style

```css
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


## 手机拍照和上传图片

```html
<!-- 选择照片 -->
<input type=file accept="image/*">
<!-- 选择视频 -->
<input type=file accept="video/*">
```

## 取消input在ios下，输入的时候英文首字母的默认大写

```html
<input autocapitalize="off" autocorrect="off" />
```

## 打电话和发短信

```html
<a href="tel:0755-10086">打电话给:0755-10086</a>
<a href="sms:10086">发短信给: 10086</a>
```