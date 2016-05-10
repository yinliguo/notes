## Cross-Origin Resource Sharing

> Cross-origin resource sharing(CORS) 允许网页上的被限制的资源（例如，字体）可以被不是资源所在的域中请求。

> 一个web网页可以自由地嵌入其它域的图片、样式文件、脚本文件、iframe、视频和一些插件内容（比如adobe flash），但是由于同源策略，字体和ajax请求被限制只能访问父页面的domain的资源。跨域ajax请求默认被阻止，因为它们能实现高级的请求（POST、PUT、DELETE和其它类型的HTTP请求，同时自定义HTTP header），从而引出跨网站脚本安全问题。

> CORS定义了一种方式，浏览器和服务器通过交互来确定是否允许跨域请求。它比同源请求更自由和实用，比简单地允许所有的跨域请求更安全。它是W3C的推荐标准。

### CORS如何工作
CORS标准描述了新的HTTP headers（提供给浏览器和服务器一个方式去请求它们有权限的远程URLs）。尽管服务器端能实现一些验证和授权，但通常是浏览器负责支持这些headers并且honor the restrictions they impose。

对于AJAX和HTTP request methods（那些能够修改数据，通常是带确定的MIME类型的、非GET和POST的方法），规范授权浏览器在发送请求之前使用HTTP OPTIONS请求去索要服务器端支持的方法，然后根据结果发送实际的请求。服务器也会通知客户端是否随请求发送资格认证（包括Cookies和HTTP认证数据）

![](https://raw.githubusercontent.com/yinliguo/notes/master/img/Flowchart_showing_Simple_and_Preflight_XHR.png)

### 与CORS相关的headers
Request headers
- Origin
- Access-Control-Request-Method
- Access-Control-Request-Headers

Response headers
- Access-Control-Allow-Origin
- Access-Control-Allow-Credentials
- Access-Control-Expose-Headers
- Access-Control-Max-Age
- Access-Control-Allow-Methods
- Access-Control-Allow-Headers

### CORS vs JSONP
CORS可以被用于现代浏览器取代JSONP。JSONP只支持GET方法，CORS支持其它的方法。CORS使web开发者能使用XMLHttpRequest，XMLHttpRequest提供了比JSONP更好的错误处理机制。另一方面JSONP支持较早的浏览器。当外部网站妥协时，JSONP会引起XSS问题，CORS允许网站手动处理结果以保证安全。

