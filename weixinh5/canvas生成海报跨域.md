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