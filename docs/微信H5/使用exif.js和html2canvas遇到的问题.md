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