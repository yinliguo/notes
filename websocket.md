## WebSocket
### Opening HandShake
#### 客户端
1、发送连接请求

```
/* ws端口默认80 */
ws-URI = "ws:" "//" host [ ":" port ] path [ "?" query ]
/* wss 端口默认443 */
wss-URI = "wss:" "//" host [ ":" port ] path [ "?" query ]
```
* 连接被设置为CONNECTING
* 如果是浏览器，应该在header中提供origin
* 如果上面的URI格式不正确，客户端应该终止连接
* 假如有一个正在请求连接正处于CONNECTING状态，不允许再请求连接到同一个主机(IP address)，必须等到前一个连接建立成功/失败为止。因此如果多个请求连接到同一个IP地址，客户端必须对其进行排序。如果不能确定远程主机的IP，那么客户端必须假定每一个主机名都指定不同的远程主机，并且应该限制同时处于挂起状态(pendding)的连接数量，例如客户端允许同时对a.example.com和b.example.com的pedding连接，但不再允许对第三个主机的连接请求。浏览器环境下需要考虑可能会有多个标签页，因此需要限制pedding连接的数量
* 服务器接受连接的数量是没有限制的，但当负载较高时能拒绝接受已经有较多连接的主机/IP的连接请求，或断开占用资源过多的连接
* 如果连接没打开，无论是因为直连失败还是代理返回错误，客户端必须标记连接为失败，且终止尝试请求
* 如果是wss，在连接打开后、发送握手数据前客户端必须进行TLS握手，如果失败了，客户端必须将连接标记为失败并终止连接。在TLS握手过程中，客户端必须使用服务器名字相关的扩展

2、客户端发送Opening Handshake

```
GET /chat HTTP/1.1
Host: a.example.com
Upgrade: websocket
Connection: Upgrade
Origin: http://www.example.com
Sec-WebSocket-Key: AQIDBAUGBwgJCgsMDQ4PEC==
Sec-WebSocket-Version: 13
Sec-WebSocket-Protocol: soap, wamp
Sec-WebSocket-Extensions: foo, bar; baz=2
Cookie: a=123
...
```

* 一旦连接建立，客户端必须发送一个opening handshake到服务器。这次握手由一个http升级协议请求组成，请求的header中有一些必要和可选的字段
* 这个请求的方法必须是GET，HTTP版本最少是1.1
* 请求的Request-URI是一个相对路径的URI或http/https URI，解析后的resource name、host、port应与ws/wss URI相匹配
* 请求的header中必须包含Host字段，如果不是默认端口还必须包含":port"
* header中必须包含Upgrade字段，字段值为"websocket"关键字
* header必须包含Connection字段，字段值为Upgrade
* header必须包含Sec-WebSocket-Key字段，是一个随机的16字节的值，然后进行base64编码。每次连接的值都不相同
* 如果是浏览器环境，header必须包含Origin字段；非浏览器环境也可能包含这个字段
* header必须包含Sec-WebSocket-Version，值必须为13
* header可能包含Sec-WebSocket-Protocol字段，值为逗号分隔的非空字符串，每个字符串必须唯一。这个字段表示子协议，优先级与定义的顺序一致
* header可能包含Sec-WebSocket-Extensions字段，表示协议级别的扩展
* 一旦发送了opening handshake，客户端在获得响应前不能发送其它数据
* 如果响应的状态码不是101，客户端就按照HTTP原理来处理结果。特殊的是，如果是401，客户端需要进行授权校验；如果是3xx，可能会重定向，客户端不需要管这些
* 如果响应的header缺少Upgrade字段，或者字段值不是非敏感的ASCII码"websocket"，客户端必须标记连接为失败
* 如果响应的header缺少Sec-WebSocket-Accept，或者字段值不等于`base64_encode(SHA-1(Sec-WebSocket-Key的值+"258EAFA5-E914-47DA-95CA-C5AB0DC85B11"))`，客户端必须标记连接失败
* 如果响应的header包含Sec-WebSocket-Extensions，并且字段值不在请求的Sec-WebSocket-Extensions中，客户端必须标记连接失败
* 如果响应中包含Sec-WebSocket-Protocol，并且字段值不在请求的Sec-WebSocket-Protocol中，客户端必须标记连接失败
* 如果响应不是按照下面定义的服务器端响应格式，就必须标记连接失败
* 如果上面的校验都通过了，就标记连接状态为OPEN

#### 服务器端
1、服务器响应Opening Handshake

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
Sec-WebSocket-Protocol: soap
Sec-WebSocket-Extensions: foo
```

* 服务器可以将连接管理转让给其他的网络代理，如负载均衡、反向代理，这时它只负责处理数据帧
* 如果客户端没有按照上面的格式进行握手，服务器返回合适的错误码，比如400 Bad Request
* 如果连接来自HTTPS，就进行TLS握手。如果请求包含server_name扩展字段，而服务器不存在，就关闭连接，否则将来的通信都基于TLS加密
* 服务器可以进行授权校验，比如返回401状态码和WWW-Authenticate字段
* 服务器可以使用3xx进行重定向，这个步骤可以与上面的授权校验同时、在它之前、在它之后进行
* 校验Origin字段。如果没通过Origin校验，服务器返回合适的状态码，如403 Forbidden
* 校验Sec-WebSocket-Key字段。进行base64_decode应该得到一个16字节的值
* 校验Sec-WebSocket-Version。如果这个值不是服务器能处理的版本，就返回合适状态码，如426 Upgrade Required和一个包含能处理的版本的Sec-WebSocket-Version字段
* 校验resource name(Request-URI)。如果请求的资源不可用，返回合适的状态码，如404 Not Found，并终止握手
* 校验Sec-WebSocket-Protocol。如果客户端发送了该字段，服务器只能从提供的值里面选择，并把响应的header中的Sec-WebSocket-Protocol字段设置为选择的值；如果无法满足任何一个协议，响应中就不能包含Sec-WebSocket-Protocol字段
* 校验Sec-WebSocket-Extensions。处理过程同Sec-WebSocket-Protocol
* 如果服务器接受了连接，就按照上面的格式进行响应，然后把连接置为OPEN状态
* Sec-WebSocket-Accept字段为`base64_encode(SHA-1(Sec-WebSocket-Key的值+"258EAFA5-E914-47DA-95CA-C5AB0DC85B11"))`


### Sending and Receiving data
* 为了避免网络中间人，如解释代理(intercepting proxies)迷惑，和安全考虑，客户端必须对所有发送到服务器的帧进行掩码处理，如果服务器发现没有做掩码处理，可能会发送1002状态码的关闭帧给客户端。服务器不能发送给客户端掩码处理的帧，如果客户端接收了掩码处理的帧，必须关闭连接，可能会发送1002状态码（这些规则将来可能会发生变化）

![帧格式](https://raw.githubusercontent.com/yinliguo/notes/master/img/websocket-frame.png)

* FIN：1bit，标记是否是最后一帧。第一帧有可能也是最后一帧
* RSV1, RSV2, RSV3：每个字段1bit，

### Closing the Connection