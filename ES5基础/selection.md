## 引言

JavaScript 提供了 Selection 这个 API 来实现选中文本的复制黏贴。

## 属性
1.anchorNode
返回选中区域对应的节点

2.anchorOffset
返回选中区域的起始下标，需要注意起始下标会根据左右方向选择的次序不同来展示不同的下标。如果 anchorNode 是字符串则对应文字下标，anchorNode 是元素，则对应选中区域对应它之前的同级节点的数目。

3.focusNode
返回选中区域终点所在的节点。

4.focusOffset
与 anchorOffset 类似，如果是 focusNode 是字符串，则对应最后一个选中的字符所在的位置，focusOffset 是元素，则对应选中区域对应同级节点的总数。

5.rangeCount
返回选中的区域所对应的连续的范围内的数量。

6.type
返回选中区域所对应的类别是连续的 range，还是同一个位置的 isCollapsed。

## 方法
1.toString
返回当前选中区域的文本内容

2.containsNode(Node)
判断某个节点是否在选区内

3.addRange
将一个区域加入到当前选区中

PS：这里只罗列几个常用的，其他属性大家可以自行去 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Selection) 上看哈。

## 使用

获取选中的内容，并复制到剪切板：
```javascript
var selection = window.getSelection();
document.addEventListener('mouseup', function() {
	if (selection.type === 'Range') {
		// 获取选区内容
		var selectedContent = selection.toString()
		// 执行复制命令
		document.execCommand('Copy')
	} else {
		console.log('you should select text!')
	}
})
```

接下来就可以黏贴了（Ctrl + V 或右键黏贴）。

****
PS：在没看 MDN 上的文档之前，我是这样获取选区的内容的（不推荐这样使用，但是可以深入了解一下 Selection 这个对象）：

```javascript
var selection = window.getSelection();
document.addEventListener('mouseup', function() {
	if (selection.type === 'Range') {
		var wholeSelectArea = selection.focusNode.wholeText
		// 开始位置
		var startInx = selection.anchorOffset
		// 结束位置
		var endInx = selection.focusOffset
		var selectedArea = startInx < endInx ? str.slice(startInx, endInx) : str.slice(endInx, startInx)
	} else {
		console.log('you should select text!')
	}
}
```
