# Ajax与Comet

## XMLHttpRequest对象

&nbsp;&nbsp;&nbsp;&nbsp;IE5是第一款引入XHR对象的浏览器，在IE中可能会有三种不同版本的XHR对象，即MSXML.XMLHttp、MSXML2.XMLHttp.3.0和MSXML2.XMLHttp.6.0

### XHR的用法

&nbsp;&nbsp;&nbsp;&nbsp;通过open()方法启动一个请求(此时请求并没有发送)，它接受三个参数：要发送的请求的类型("get"、"post"等)、请求的URL和表示是否异步发送请求的布尔值。
```
	xhr.open("get", "example.php", false)
```
	
&nbsp;&nbsp;&nbsp;&nbsp;调用send()方法接受一个参数，即作为请求主体发送的数据，如果不需要则需传入null。然后请求就会分派到服务器。

&nbsp;&nbsp;&nbsp;&nbsp;调用send()方法接受一个参数，即作为请求主体发送的数据，如果不需要则需传入null。然后请求就会分派到服务器。
在收到响应后，响应的数据会自动填充XHR对象的属性。其属性简介如下：
- responseText   响应的文本
- responseXML	   如果响应的内容类型是"text/xml"或"application/xml"，则这个属性会包含响应数据的XMLDOM文档
- status		   响应HTTP状态  200代表成功 304代表请求的资源没有被修改
- statusText	   HTTP响应状态的说明
&nbsp;&nbsp;&nbsp;&nbsp;当opne()方法的第三个参数为false时，即代表启动的是异步请求，此时就可以检测XHR对象的readyState属性，该属性表示请求/响应过程的当前活动阶段。
&nbsp;&nbsp;&nbsp;&nbsp;readyState属性可取的值：
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0: 为初始化 尚未调用open()方法
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1: 启动	已经调用open()方法，但是没有调用send()方法
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2: 发送	已经调用send()方法，但尚未接收响应
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3: 接收  已经接收到部分响应数据
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4: 完成	已经接收全部响应数据，而且已经在客户端使用
&nbsp;&nbsp;&nbsp;&nbsp;当readyState属性的值由一个值变成另一个值时，会触发一次readystatechange事件。
&nbsp;&nbsp;&nbsp;&nbsp;例如：
```
	var xhr = createXHR();
	xhr.onreadystatechange = function(){
		if(xhr.readyState == 4){
			if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
				alert(xhr.responseText);
			}else{
				alert("Request was unsuccessful:" + xhr.status);					
			}
		}
	}
	xhr.open("get", "example.txt", true);					
	xhr.send();
```
&nbsp;&nbsp;&nbsp;&nbsp;注：xhr.about()可以取消异步请求

## HTTP头部信息

&nbsp;&nbsp;&nbsp;&nbsp;XHR提供了两种头部，请求头部和响应头部。
&nbsp;&nbsp;&nbsp;&nbsp;默认情况下，在发送XHR请求的同时，会发送下列头部信息：
```
	Accept:	 浏览器能够处理的内容类型
	Accept-Charset:	 浏览器能够显示的字符串集
	Accept-Encoding:  浏览器能够显示的字符集
	Accept-Language:	浏览器当前设置的语言
	Connection:		浏览器与服务器之间连接的类型
	Cookie:		当前页面设置的任何cookie
	Host:	发出请求的所在的域
	Referer:	发送请求的页面URI
	User-Agent:	 浏览器的用户代理字符串
```

## GET请求

&nbsp;&nbsp;&nbsp;&nbsp;Get请求通常是将查询字符串参数追加到URL末尾，实现向服务器发送请求。
&nbsp;&nbsp;&nbsp;&nbsp;例如：  
```
	var xhr = createXHR();
	xhr.open("get", "example.php?name1=wjc&age=21", true);
```

## POST请求

&nbsp;&nbsp;&nbsp;&nbsp;POST请求通常用于向服务器发送应该被保存的数据。
&nbsp;&nbsp;&nbsp;&nbsp;例如： 
```
	var xhr = createXHR();
        xhr.open("post", "example.php", true)
```

### POST请求模拟表单提交
	
&nbsp;&nbsp;&nbsp;&nbsp;XMLHrrpRequest2级定义了FormData类型，用于表单序列化和创建与表单格式相同的数据。
&nbsp;&nbsp;&nbsp;&nbsp;创建FormData对象方法1：
```
	var data = new FormData();
	data.append("name", "wjc");
```

&nbsp;&nbsp;&nbsp;&nbsp;方法2：
```
	//获取文档中的表单元素
	var form = document.getElementById("user-info");
	//添加参数
	//实例FormData对象
	xhr.send(new FormData(form));
```

### 跨域源资源共享

&nbsp;&nbsp;&nbsp;&nbsp;XHR实现Ajax通信不能进行跨域间的通信。XHR对象只能访问与包含它的页面位于同一个域的资源。

&nbsp;&nbsp;&nbsp;&nbsp;CORS(Cross-Origin Resource Sharing, 跨域源资源共享)，是W3C定义的必须访问跨资源时，浏览器与服务器应该如何沟通。
&nbsp;&nbsp;&nbsp;&nbsp;核心概念：使用自定义的HTTP头部让浏览器与服务器进行沟通，决定请求或响应是否应该成功。

&nbsp;&nbsp;&nbsp;&nbsp;简单的使用GET或POST请求是没有自定义的头部，主体内容时text/plain。如果需要发送跨域请求，需要额外的加一个Origin头部，其中包含请求页面的源信息（协议、域名和端口），如何服务器工具这个头部来决定是否给予响应。
&nbsp;&nbsp;&nbsp;&nbsp;例如： 
		  Origin: http://www.mczoline.net
		  如果服务器认为请求可以接收，则会在Access-Control-Allow-Origin头部中回发相同的源信息
		  Access-Control-Allow-Origin: http://www.nczonline.net

#### IE对CORS的实现

&nbsp;&nbsp;&nbsp;&nbsp;微软的IE8中引入XDR（XDomainRequest）类型。和XHR类似，可以是西安安全可靠的跨域通信。
&nbsp;&nbsp;&nbsp;&nbsp;与XHR的不同：
				cookie不会随请求发送，也不会响应返回。
				只能设置请求头部信息中的Content-Type字段
				不能访问响应头部信息
				只支持GET和POST请求
				
&nbsp;&nbsp;&nbsp;&nbsp;XDR对象的使用方法和XHR对象的使用方法类似，不同与它的是创建的是XDomainRequest的实例，并且XDR对象的open()方法只接收两个参数：请求类型和URL
&nbsp;&nbsp;&nbsp;&nbsp;XDR的请求都是异步执行，请求返回后，会触发load事件，同样的数据也会保存在responseText属性中。

#### 其他浏览器对CORS的实现

&nbsp;&nbsp;&nbsp;&nbsp;其他浏览器都是通过创建XMLHttpRequest对象实现了对CORS的原生支持。

## 其他跨域技术

### 图像Ping
&nbsp;&nbsp;&nbsp;&nbsp;图像Ping是服务器进行监督、单向的跨域通信的一种方式。请求的数据是通过查询字符串形式发送的，而响应可以是任意内容，通常是像素图或204响应。

### JSONP

&nbsp;&nbsp;&nbsp;&nbsp;JSONP是JSON with padding(填充式JSON或参数式JSON)的简写，是应用JSON的一种新方式，在后来的Web服务中非常流行。（可以理解为被包含在函数调用的JSON）
&nbsp;&nbsp;&nbsp;&nbsp;例如：callback({"name", "wjc"});

&nbsp;&nbsp;&nbsp;&nbsp;JSONP由两部分组成：回调函数和数据。回调函数是当响应到来时应该在页面中调用的函数，回调函数名一般在请求中指定。
数据是传入回调函数中的JSON数据。例如： http://freegeip.net/json/?callback=handleResponse

&nbsp;&nbsp;&nbsp;&nbsp;JSONP可以通过和动态的script元素一起使用，可以为script的src属性指定一个跨域URL，即和img元素类似
&nbsp;&nbsp;&nbsp;&nbsp;优点：在于能直接发我问响应文本，支持在浏览器与服务器之间双向通信。
&nbsp;&nbsp;&nbsp;&nbsp;缺点：从其他域加载代码，会存在一定风险。

### Comet

&nbsp;&nbsp;&nbsp;&nbsp;Comet是Alex Russell发明的词，指更高级的Ajax技术（也成为“服务器推送”）。
&nbsp;&nbsp;&nbsp;&nbsp;Comet适合用于处理体育比赛地分数1和股价报价。
