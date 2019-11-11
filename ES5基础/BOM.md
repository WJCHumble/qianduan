# BOM

## 窗口大小
	由于各个浏览器的兼容性的问题，获取浏览器的窗口大小较为麻烦，但是获取可视的窗口大小是可以获取的，例如：
	var pageWidth = window.innerWidth,
		pageHeight = window.innerHeight;

	//兼容浏览器
	if(typeof pageWidth != "number"){
		//document.compatMode判断文档是否处于标准模式(CSS1Compat) 	IE必须在标准模式下  document.documentElement.clientWidth才有效
		if(document.compatMode == "CSS1Compat"){
			pageWidth = document.documentElement.clientWidth;
			pageHeight = document.documentElement.clientHeight;
		}else{
			//chrome无论什么模式都可以的
			pageWidth = document.body.clientWidth;
			pageHeight = document.body.clientHeight; 
		}
	}

## location对象
	location是最有用的BOM对象之一，提供了与当前窗口中加载的文档有关的信息。

	属性：
		hash		"#contents"
		host		"www.wrrox.com:80"
		hostname	"www.wrox.com"
		href		"http:/www.wrox.com"
		pathname	"/WileyCDA/"
		port		"8080"
		protocol	"http:"
		search      "?q=javascript"

	位置操作
	location很多方法都可以实现对浏览器位置（地址）的改变
	location.assign("http://www.wrox.com");
	location.href = "http://www.wrox.com"
	.....

	补充：个人觉得reload()方法也比较有用，可以实现强制从服务器重新加载（调试的时候去缓存非常有效）
			location.reload(true)  //重新加载  强制从服务器重新加载

## 间歇调用和超时调用

	超时调用即setTimeOut()，它接收两个参数，要执行的代码（一般写函数）和毫秒表示的时间（即在执行代码前需要等待多少毫秒）

	setTimeOut(function(){}, time)

	间歇调用即setInterval()，它接收两个参数，要执行的代码（一般写函数）和毫秒表示的时间（即间隔多少时间执行一次）

	setInterval(function(){}, time)

	注：无论是间歇还是超时，在执行后都需要清除即clearInterval(name) clearTimeOut(name)

## navigator对象
	
	属性或方法：
		plugins		浏览器中安装的插件信息的数组 
		useAgent	获取浏览器的用户代理字符串（浏览器内核）

	检测插件
		plugins数组包含的属性：
			name			插件的名字
			description		插件的描述
			filename		插件的文件名
			length			插件所处理的MIME类型数量
		//检测插件 IE无效
		function hasPlugin(name){
			name = name.toLowerCase();
			for(var i = 0; i < navigator.plugins.length; i++){
				if(navigator.plugin[i].name.toLowerCase().indexOf(name) > -1){
					return true;
				}
			}
			return false;
		}
		//检测Flash
		alert(hasPlugin("Flash"))
		//检测QuickTime
		alert(hasPlugin("QuickTime"));

## screen对象

	screen在JavaScript中用处不大，availWith和availHeight会使用到，是检减去系统不见得高度之后的高，宽同理

## history对象

	history保存着用户上网的历史记录，从窗口被打开的那一刻算起。
	常见操作：history.go(-1)    回退一页
			 history.go(1)     前进一页
			 history.go(2)     前进两页
			 history.back()    前进一页
			 history.forward() 后退一页