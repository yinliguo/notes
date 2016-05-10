## Web Attack

### Cross-site request forgery
跨站请求伪造。利用浏览器去给一个认证过的网站发请求，由于认证过，所以被访问的网站以为这个用户行为，

##### 例子
一家银行的转账地址是 http://www.examplebank.com/withdraw?account=AccoutName&amount=1000&for=PayeeName ，在另一个网站上放一个链接
```
<a href="http://www.examplebank.com/withdraw?account=Alice&amount=1000&for=Badman">
```
如果一个用户登录完银行，然后又点击了这个恶意网站的这个url，那么就中招了。

##### 分析
CSRF不能获取用户的信息，也不能直接操作用户，只是利用了简单的身份认证实现了用户行为的操作

##### 防御
###### 1. 检查Referer字段
这种方法比较简单，只需要判断header中的Referer字段是否在白名单中即可，但是这个字段很容易被恶意修改，所以不太保险

###### 2. 添加校验Token
使用一个随机字符串作为请求的一个参数，服务器对其进行校验

-------------------------

### XSS
跨站脚本攻击。通过将恶意代码注入到网页中，来获取用户信息或执行一些操作。

##### 例子
恶意用户发布了一篇文章，里面嵌入了恶意代码，当别人打开这篇文章时，就会触发恶意代码。

##### 分析
XSS攻击主要是由用户输入中携带恶意代码造成的，需要对用户输入进行过滤

##### 防御
- 将用户输入中的特殊字符，比如\<script>、\<iframe>过滤掉
- 对输入的内容进行HTML Encode
- 对URL进行encode

-------------------------

### SQL injection
SQL注入。




