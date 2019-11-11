# DOM

	每一个html原素都有id title lang dir className等特性

## 以下列举几个常用的方法

### getAttribute()

	可以获取相应元素的指定特性

### setAttribute()

	可以自定义元素的特性或设置已有特性的值

### removeAttribute()

	可以移除元素的指定特性

### document.createElement(TagName)
	
	通过指定标签名(TagName)来动态的创建元素

### appendChild()
	
	可以为元素添加相应的子标签

### document.createTextNode(String)
	
	可以创建文本标签（注文本标签在html中就是指文本，并不是p标签之类的）

### chileNodes[]
	
	获取元素的子节点，使用该方法每一个元素的子节点都是以数组的形式返回

### firstChild

	获取元素的第一个子节点

## 补充：文档片段DocumentFragment类型(重点)
	
	在所有的节点类型中，只有DocumentFragment在文档中没有对应的标记，DOM规定文档片段是一种“轻量级”的文档，可以包含和控制节点，但不会相完整的文档那样占用额外的资源。它具有以下特征：
						nodeType值为: 11
						nodeName的值为： "#document-fragment"
						nodeValue的值为:	 null
						parentNode的值为: null
	用于动态渲染页面时documentfragment非常有效
	例如：
		var fragment = document.createDocumentFragment();
		var ul = document.getElementById("myList");
		var li = null;

		for(var i = 0; i < 3; i++){
			li = document.createElement('li');
			li.appendChild(document.createTextNode("text" + (i + 1)));
			fragment.appendChild(li);
		}

		ul.appendChild(fragment);
	个人认为vue的v-for渲染机制很可能借用了这个

# DOM扩展

## 选择符API(querySelector())
	
	诸多JavaScript库都是根据CSS选择符实现与DOM元素的匹配（例如Juqery和核心就是通过CSS选择符查询DOM文档	取得元素的引用）

	Selectors API Level 1的核心是两个方法：querySelector() 和 querySelectorAll()

### querySelector()方法

	querySelector()方法接收一个CSS选择符，返回和该模式匹配的第一个元素，没有则返回null
	例如：
		var body = document.querySelector("body");
		var div1 = document.querySelector(".div");
		var div1 = document.querySelector("img.img1")

### querySelectorAll()方法
	
	querySelectorAll()方法和querySelector()方法一致，只是他返回的是一个NodeList的实例
	例如：
		var liS = document.getElementById("myList").querySelectorAll("li");
		console.log(liS);
	即返回一个节点数组

### matchesSelector()方法
	
	Selectors API Level2 规范为Element类型新增了一个方法matchesSelector()方法。同样是接受一个CSS选择符，匹配则返回true，否则返回false