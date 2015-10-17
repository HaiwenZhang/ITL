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

