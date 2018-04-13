# Node JS

## server

### 区分2个概念：Web Server和Application Server

* Web Server:
> 当Web Server接收到一个HTTP request的时候，它会以HTTP response的形式相应这个请求，也就是返回一个HTML页面， Web Server可以响应一个静态的HTTP页面，也可以转发或者代理请求到其他的服务端脚本引擎(CGI, JSP或者ASP、PHP、Node.js等等)，然后返回一个动态的相应。不管以什么样的服务端技术， Web Server大多说情况都只是以HTML德形式返回一个HTTP响应。
* Application Server
> 根据Application Server的定义， Application Server是为客户端应用提供业务逻辑，它与客户端应用的交互可以通过多种协议，其中也包括HTTP协议， 一个Web Server主要是处理HTTP请求，发送HTML到浏览器，而Application Server为客户端应用提供了访问业务逻辑的接口。客户段应用可以像调用一个对象的方法一样调用这些业务逻辑。