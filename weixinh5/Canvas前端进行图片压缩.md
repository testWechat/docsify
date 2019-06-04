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