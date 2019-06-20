```js
/**
 * @param {Number} box_w 固定盒子的宽, box_h 固定盒子的高
 * @param {Number} source_w 原图片的宽, source_h 原图片的高
 * @return {Object} {截取的图片信息}，对应drawImage(imageResource, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)参数
 */

function coverImg(box_w, box_h, source_w, source_h) {
	var sx = 0,
		sy = 0,
		sWidth = source_w,
		sHeight = source_h;
	if (source_w > source_h || (source_w == source_h && box_w < box_h)) {
		sWidth = box_w * sHeight / box_h;
		sx = (source_w - sWidth) / 2;
	} else if (source_w < source_h || (source_w == source_h && box_w > box_h)) {
		sHeight = box_h * sWidth / box_w;
		sy = (source_h - sHeight) / 2;
	}
	return {
		sx, sy, sWidth, sHeight
	}
}


/**
 * @param {Number} sx 固定盒子的x坐标,sy 固定盒子的y左标
 * @param {Number} box_w 固定盒子的宽, box_h 固定盒子的高
 * @param {Number} source_w 原图片的宽, source_h 原图片的高
 * @return {Object} {drawImage的参数，缩放后图片的x坐标，y坐标，宽和高},对应drawImage(imageResource, dx, dy, dWidth, dHeight)
 */

function containImg(sx, sy, box_w, box_h, source_w, source_h) {
	var dx = sx,
		dy = sy,
		dWidth = box_w,
		dHeight = box_h;
	if (source_w > source_h || (source_w == source_h && box_w < box_h)) {
		dHeight = source_h * dWidth / source_w;
		dy = sy + (box_h - dHeight) / 2;

	} else if (source_w < source_h || (source_w == source_h && box_w > box_h)) {
		dWidth = source_w * dHeight / source_h;
		dx = sx + (box_w - dWidth) / 2;
	}
	return {
		dx, dy, dWidth, dHeight
	}
}
```