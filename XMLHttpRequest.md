## XMLHttpRequest

> XMLHttpRequest是一个获取资源的API

### 构造函数
```
var client = new XMLHttpRequest();
```
- 1. 生成一个新的XMLHttpRequest对象xhr
- 2. Set xhr's settings object to the relevant settings object for the global object of xhr's interface object.
- 3. 返回xhr

### 事件处理
下面是事件处理器和对应的事件类型，依赖于一个实现了XMLHttpRequestEventTarget接口的对象，这些事件处理器作为这个对象的属性
|event handler  |event type |
|---------------|:---------:|
|onloadstart	|loadstart	|
|onprogress		|progress	|
|onabort		|abort		|
|onerror		|error		|
|onload			|load		|
|ontimeout		|timeout	|
|onloadend		|loadend	|

下面的事件处理器作为一个XMLHttpRequest的属性
|event handler	  |event type	   |
|-----------------|:--------------:|
|onreadystatchange|readystatechange|
