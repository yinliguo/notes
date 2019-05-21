## One http request
### 域名解析
如果请求地址是域名，就会进行DNS解析，将域名解析成ip
### 与服务器建立TCP连接
#### 1、客户端发送SYN(Synchronize)(seq=n)
#### 2、服务器响应ACK(Acknowlege)(seq=n, ack=1)
#### 3、客户端发送ACK(seq=n+1, ack=1)
### 发送http请求
```
GET / HTTP/1.1
Host: 192.168.199.220:8000
Accept: text/html
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.1.1 Mobile/15E148 Safari/604.1

```
### 服务器返回响应
```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 191

<!DOCTYPE html>
...
```
> 下面是页面渲染过程

#### 1、DOM（文档对象模型）解析
![dom process](https://github.com/yinliguo/notes/blob/master/img/dom-process.png?raw=true)
#### 2、CSSOM（CSS对象模型）解析
![](https://github.com/yinliguo/notes/blob/master/img/cssom-construction.png?raw=true)
![](https://github.com/yinliguo/notes/blob/master/img/cssom-tree.png?raw=true)

* 默认情况下，CSS阻塞渲染（指浏览器暂停网页的首次渲染，直到该资源准备完毕），所以应尽早尽快地将下载到客户端，以便缩短首次渲染的时间
* 可以使用媒体查询减少阻塞的CSS文件。例如`<link href="print.css" rel="stylesheet" media="print">`。注意：浏览器仍然会下载这些文件，只是这些文件的优先级较低
#### 3、渲染树
![](https://github.com/yinliguo/notes/blob/master/img/render-tree-construction.png?raw=true)

1. 从DOM树的根节点开始遍历每个*可见*节点（包括脚本标签等本身就不可见的节点和被css隐藏的节点，注意visibility: hidden不同于display: none，前者会渲染成空框，而后者从渲染树中移除）
2. 对每个可见节点，找到适配的CSSOM规则并应用
3. 生成可见节点，连同其内容和计算的样式

#### 4、Layout
计算渲染树的每个节点在浏览器中的大小和位置
#### 5、Paint
把渲染树的每个节点绘制到屏幕上

#### JavaScript
* js会组织构建DOM构建
* 必须等待CSSOM下载和构建之后才能执行js脚本
* <script>加上async属性不阻止DOM构建
* 