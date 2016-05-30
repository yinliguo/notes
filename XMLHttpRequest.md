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
|event handler  |event type |
|---------------|:---------:|
|onloadstart	|loadstart	|
|onprogress		|progress	|
|onabort		|abort		|
|onerror		|error		|
|onload			|load		|
|ontimeout		|timeout	|
|onloadend		|loadend	|