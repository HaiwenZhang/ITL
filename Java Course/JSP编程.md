#<center>JSP编程
#1、B/S 与 C/S架构
Browser <——> Server 与  Client <——>Server

#2、网页运行流程
##1)URL
```
http://jsjxy.shiep.edu.cn/index.asp
```

##2)Server
web服务器(IIS6.0): 

######接受URL, 定位查找URL资源的物理位置
######如果是静态资源(HTML, 图片等), 直接发送到客户端的浏览器上
######如果是动态资源(动态页面) eg: asp, jsp, php....
######编译，执行，将结果以text/html格式发送到浏览器。
######客户端的浏览器解释执行html源代码,呈现结果。

#3、HTML、CSS、JavaScript
##1)HTML5
```
<html>
<head>
	<title>......</title>
</head>
<body>
	
</body>
</html>

```

###a)字体标记
```
 <h1>,..., <h6>
```

###b)表单form
```
<form action="reg.jsp" methd="post">
	<input type="text" name="uid">
	<input type="password" name="password">
	<input type="radio" name="usex" value="m">男
	<input type="radio" name="usex" value="w">女
	<input type="checkbox" name="uhobby" value="sports">体育
	<input type="checkbox" name="uhobby" value="music">音乐
	<select name="ucity">
		<option value="021">上海</option>
		<option value="022">北京</option>
	</select>
	<input type="submit" value="注册">
</form>
```

##2)JavaScript
运行在浏览器中的脚本，浏览器解释执行JavaScript.

```
 <script type="text/javascript">
	vat s = 1;
	for(vat i=1; i<=10:i++) {
		s *= i;
		document.write( i+ "!=" + s + "<br>")
	}
  </script>
```
##3)CSS层叠样式表
DIV+CSS布局(960CSS)

#4、JSP基础
##1)、page指令
```
<%@ page language="java" pageEncoding="gbk" %>
<%@ page import="java.util.*, dao.CustomerDao" %>
```
设置错误处理页面

```
//errhandle.jsp
<%@ page language="java" isErrorPage="true" %>
<%@ page errorPage="errhandle.jsp" %>
```

##2)、内置对象
######out;request;response

```
String uid = request.getParameter("uid");
response.sendRedirect("login.jsp");
response.sendRedirect("http://www.shiep.edu.cn");
```

######pageContext, session, application:服务器端存储的内置对象
```
pageContext.setAttribute("count", new Integer(100));
Object obj = pageContext.getAttribute("count");
pageContext.removeAttribute("count");
eg:
<%
	Object obj = session.getAttribute("counter");
	if(obj == null) {
		session.setAttribute("counter", new Integer(1));
	} else{
		int x = ((Integer)obj).intValue() + 1;
		session.setAttribute("counter", new Integer(x));
	}
 %>
 <h1>你是第<%=session.getAttribute("counter") %>个访问者</h1>
```

#5、JavaBean MVC
JavaBean是Java类，实现业务层代码;

