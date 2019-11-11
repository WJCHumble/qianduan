# Canvas

	基本用法：添加一个canvas，设置宽高，canvas元素.geContext测试浏览器是否支持canvas元素

## 2D上下文
	
	可以实现矩形、弧线、路径的绘制。

### 填充和描边

	填充和描边的结果取决于两个属性:fillStyle填充和strokeStyle描边
	例如：    2d上下文.fillStyle = "#aaf";
			 2d上下文.strokeStyle = "#faa"

### 绘制矩形
	
	与绘制矩形相关的方法包括：fillRect()填充矩形、strokeRect()矩形描边、clearRect()清除指定位置与大小的矩形。
	每一个方法都接收了4个参数：矩形的x轴坐标、y轴坐标、宽度、高度。
	例如：
		//绘制填充矩形
		context.fillRect(10, 10, 50, 50);
		//绘制描边矩形
		context.strokeReect(10, 10, 50, 50);

### 绘制路径

	通过路径可以实现复杂的形状和线条。
	首先需要调用beginPath()方法，表示要开始绘制新路径。然后再通过调用相应的方法实际的绘制路径：
	arc(x, y, radius, startAngle, endAngle counterclockwise)

	arcTo(x1, y1, x2, y2, radius)

	bezierCurveTo(c1x, c1y, c2x, x, y)

	lineTo(x, y)

	moveTo(x, y)

	quadraticCurveTo(cx, cy, x, y)

	rect(X, Y, width, height)