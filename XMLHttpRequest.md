## XMLHttpRequest

> XMLHttpRequest是一个获取资源的API
> XMLHttpRequest对象最初是作为WATHWG的HTML工作的一部分（基于微软多年以前的实现），2006年被移入W3C。直到2011年底，XMLHttpRequest的一些扩展（比如progress事件和跨域请求）才在一个单独的草案（XMLHttpRequest Level 2）中被开发出来，这时候把两个规范合并起来，XMLHttpRequest再次成为一个单独的实体。2012年底被移回WHATWG。

### 构造函数
```
var client = new XMLHttpRequest();
```
- 1. 生成一个新的XMLHttpRequest对象xhr
- 2. Set xhr's settings object to the relevant settings object for the global object of xhr's interface object.
- 3. 返回xhr

### 接口
![接口](https://raw.githubusercontent.com/yinliguo/notes/master/img/XMLHttpRequest%20Standard.png)

### 事件处理
下面是事件处理器和对应的事件类型，依赖于一个实现了XMLHttpRequestEventTarget接口的对象，这些事件处理器作为这个对象的属性

|event handler  |event type |描述
|---------------|:---------:|:----------------------:|
|onloadstart	|loadstart	|开始请求				  |
|onprogress		|progress	|正在传输数据				 |
|onabort		|abort		|请求被终止				  |
|onerror		|error		|请求失败				   |
|onload			|load		|请求已完成（成功）			|
|ontimeout		|timeout	|请求超时				   |
|onloadend		|loadend	|请求已完成（无论成功或失败）	|

下面的事件处理器作为一个XMLHttpRequest的属性

|event handler	  |event type	   |
|-----------------|:--------------:|
|onreadystatchange|readystatechange|

### 状态
XMLHttpRequest有不同的状态，使用readyState属性可以返回当前的状态
- UNSENT(0): XMLHttpRequest对象已经被创建
- OPENED(1): open()方法已成功调用。在这个状态中，可以使用setRequestHeader()设置请求头
- HEADERS_RECEIVED(2): 所有的跳转都已经跳转完成，最终响应的header已经被接收，Several response members of the object are now available.（没看懂）
- LOADING(3): 响应的主体正在被接收
- DONE(4): 数据传输已经完成，或在传输过程中出错（例如循环跳转）

### 请求
每个XMLHttpRequest都有如下的概念：
- 请求的方法
- 请求的url
- 请求头部
- 请求主体
- source origin
- 请求来源地址
- 异步标志
- 上传完成标志
- 上传事件标志

终止请求的步骤：
- 1. 设置error log
- 2. 取消由XMLHttpRequest对象打开的所有请求实例
- 3. 移除任务队列中该对象的任务源

##### open()方法
```
client.open(method, url [, async = true [, username = null [, password = null]]])
```

##### setRequestHeader()方法
添加到用户请求头
```
client.setRequestHeader(header, value)
```

##### timeout属性
超时时间，毫秒数，默认为0
```
client.timeout
```

##### withCredentials属性
在跨域请求中包含用户资格认证时为true，否则为false，默认为false。当XHR的状态是UNSENT或OPENED时进行设置，否则就会抛出InvalidStateError
```
client.withCredentials
```

##### upload属性
返回关联的XMLHttpRequest对象。当数据传送到服务器端时，能用于收集传输信息

##### send()方法
开始发送请求，可选的参数是请求的主体，如果是GET或HEAD请求可以忽略该参数
```
client.send([data = null])
```

##### abort()方法
取消所有的网络活动
```
client.abort()
```
### 响应
##### status属性
返回http状态码
```
client.status
```

##### statusText属性
返回http状态文本
```
client.statusText
```

##### getResponseHeader()方法
获取响应头
```
client.getResponseHeader(header)
```

##### getAllResponseHeaders()方法
获取所有响应头
```
client.getAllResponseHeaders()
```

##### overrideMimeType()方法
为响应设置Content-Type头信息
```
client.overrideMimeType(mime)
```

##### responseType属性
返回响应的类型
- ""
- "arraybuffer"
- "blob"
- "document"
- "json"
- "text"
```
client.responseType
```

##### response属性
返回响应的主体
```
client.response
```

##### responseText属性
返回文本响应的主体
```
client.responseText
```

##### responseXML属性
返回文档响应的主体
```
client.responseXML
```

### 垃圾回收
当一个XMLHttpRequest对象的状态是opened、headers received或loading时，它不能被垃圾回收，并且它注册了一个或多个事件监听器，例如readystatechange、progress、abort、error、load、timeout、loaded。如果连接还是打开的，要想对这个XMLHttpRequest对象进行垃圾回收，客户端必须终止这个请求。


### 引用资料
- [XMLHttpRequest Living Standard](https://xhr.spec.whatwg.org/)