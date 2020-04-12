## fs模块

fs模块是唯一一个同时提供同步和异步API的模块。
例如：
同步获取当前目录的文件列表
```javascript
	console.log(require('fs').readdireSync(__dirname));
```

异步获取当前目录的文件列表
```javascript
	function async (err, files) {console.log(files)};
	requrie('fs').readdirSync('.', async);
```

## 流（stream）

process全局对象中包含了三个流对象，分别对应三个UNIX标准流：
- stdin: 标准输入	可读流
- stdout: 标准输出   可写流
- stderr: 标准错误   可写流
	
## 重构

在开发过程中，可能存在这样的情况，随着函数量的增长，过多的函数嵌套会让程序的可读性变差。为了避免这类问题，我们可以为每一个异步操作预先定义一个函数。

## 标准的读取文件的代码

```javascript
var fs = require('fs'),
	stdin = process.stdin,
	stdout = process.stdout;

fs.readdir(process.cwd(), function (err, files) {
	console.log('');    //输出一个空行

	if(!files.length) {	  //如果files数组为空，则告知用户当前目录没有文件  \n是换行
		return console.log('   \033[31m No files to show!033[39m\n]]');
	}

	console.log('   Select which file or directory you want to see\n');

	//定义一个函数，数组中每个元素都会执行该函数
	function file(i) {
		//先获取文件名
		var filename = files[i];

		//再查看文件名对应路径的情况
		fs.stat(__dirname + '/' + filename, function (err, stat) {
			if (stat.isDirectory()) {
				console.log('    ' + i + '   \033[36m' +  filename + '/\033[39m');
			} else {
				console.log('    ' + i + '   \033[90m' + filename + '\033[39m');
			}

			//计数器不断增加，检查是否还有未处理的文件
			i++;
			if (i == files.length) {
				read();
			} else {
				file(i);
			}
		});
	} 

	file(0);

	function read () {
		console.log('');
		//输出
		stdout.write('    \033[33mEnter your choice: \033[39m');
		//等待输入
		stdin.resume();
		stdin.setEncoding('utf-8');
		//用户输入后，开始监听其data事件
		stdin.on('data', option);
	}

	function option (data) {
		//获取该文件名
		var filename = files[Number(data)];
		//如果文件数组中对应索引的文件不存在
		if(!files[Number(data)]) {
			//提示再次输入索引
			stdout.write('    \033[31mEnter your choice: \033[39m');
		} else {
			stdin.pause() ;
			//指定编码，读取文件
			fs.readFile(__dirname +  '/' + filename, 'utf8', function (err, data) {
				console.log('');
				//利用正则表达式，添加一些辅助缩进
				console.log('\033[90m' + data.replace(/(.*)/g, '     $1') + '\033[39m');
			})
		}
	}

});
```

读取文件目录(readdir)：
```javascript
	fs.readdir(__dirname +  '/' + filename, function (err, files) {
		console.log('');
		console.log('    (' + files.length + ' files');
		files.forEach(function (file) {
			console.log('      -      ' + file);
		});
		console.log('');
	})
```

读取文件内容(readFile)：
```javascript
	fs.readFile(__dirname +  '/' + filename, 'utf8', function (err, data) {
		console.log('');
		//利用正则表达式，添加一些辅助缩进
		console.log('\033[90m' + data.replace(/(.*)/g, '     $1') + '\033[39m');
	})
```		

## 工作目录

- __dirname 用于获取执行文件时该文件系统所在的目录。
- process.cwd()获取当前工作目录
- process.chdir()用于切换工作目录

## 环境变量

Node提供了process.env来访问shell环境下的环境变量。


## 退出

当程序发生错误时，使用node的方式进行退出是最好的解决方案。
例如：
```javascript
	process.exit(1)
```


## 信号
	
进程与操作系统进行通信的其中一种方式是通过信号。
例如：
```javascript
	要让进程终止，发送SIGKILL信号
    process.on('SIGKILL', function () {
    	//信号已收到
	})
```

## ANSI转义码

要在终端下控制格式、颜色以及其他输出选项，可以使用ANSI转义码。
例如：
```javascript
	console.log(' \033{90m' + data.replace(/(.*)/g, '       $i') + '\033[39m');
```

- 033表示转义序列的开始
- [ 表示开始颜色设置
-  90 表示前景色为亮灰色
- m 表示颜色设置结束
- 39 将颜色恢复默认值

[完整的ANSI转义码表](http://en.wikipedia.org/wiki/ansi_escape_code)

## fs(文件系统)

fs模块允许你通过Stream API来对数据进行读写操作。（对内存的分配不是一次完成的）

### Stream

fs.createReadStream方法允许为一个文件创建一个可读的Stream对象
以流方式读写文件，可以大大提高程序运行效率，避免readFile这类方法必须读取完整个文件才能触发回到函数的情况。
例如：
```javascript
	var stream = fs.createReadStream('my-file.txt', 'utf8');

	stream.on('data', function(chunk) {
		console.log('文件开始读取');
		//处理文件部分内容
		console.log(chunk);
	});
	stream.on('end', function(chunk) {
		//文件读取完毕
		console.log('文件读取完毕');
	});
```

### 监视

Node提供了监视文件或目录是否发生变化的功能。监视意味着当文件系统中的文件发送变化时，会分发一个事件，然后触发指定的回调函数。
例如：
```javascript
	var fs = require('fs'),
		files = fs.readdirSync(process.cwd());

	files.forEach(function (file) {
		//监听'.css'后缀的文件
		if (/\.css/.test(file)) {
			fs.watchFile(process.cwd() + '/' + file, function () {
				console.log(' - '  + file + ' changed!');
			})
		}
	});
```
这个段程序，当后缀为.css的文件发生变化时，就会触发回调事件输出修改的文件名changed!	
除此之外fs文件系统还提供了fs.watch来监听整个目录。



