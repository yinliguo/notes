# WebSocket
## Opening HandShake
### 客户端
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

### 服务器端
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


## Sending and Receiving data
* 为了避免网络中间人，如解释代理(intercepting proxies)迷惑，和安全考虑，客户端必须对所有发送到服务器的帧进行掩码处理，如果服务器发现没有做掩码处理，可能会发送1002状态码的关闭帧给客户端。服务器不能发送给客户端掩码处理的帧，如果客户端接收了掩码处理的帧，必须关闭连接，可能会发送1002状态码（这些规则将来可能会发生变化）

### 帧格式
![帧格式](https://raw.githubusercontent.com/yinliguo/notes/master/img/websocket-frame.png)

* **FIN**: 1bit，标记是否是最后一帧。第一帧有可能也是最后一帧
* **RSV1**, **RSV2**, **RSV3**: 每个字段1bit，且都为0，除非协商好了使用扩展并定义了非0的值。如果收到了非0的值但是没有使用扩展或者与扩展定义的值不同，接收端必须标记连接失败。
* **opcode**: 4bits， 定义了Payload data的说明，如下

```
%x0 继续
%x1 文本
%x2 二进制
%x3-7 预留给将来的非控制帧
%x8 关闭连接
%x9 ping
%xA pong
%xB-F 预留给将来的控制帧
```
* **MASK**: 1bit, 定义Payload data是否掩码处理，如果为1，需要用 Masking-key解码数据，从客户端发到服务器的都是1
* **Payload len**, **Extended payload length**: 7bits，7+16bits或7+64bits。标识Payload Data的长度。如果Payload len是0-125，这就是Payload Data的字节数；如果Payload len=126，接下来的2个字节解释为16位无符号数字，作为Payload Data的长度(字节数)；如果Payload len=127，接下来的4个字节解释为无符号数字，作为Payload Data的长度。多子节长度按照网络字节序列排列。如果是Payload Data是124字节，Payload len就是124，不能设置成126，再占用Extended payload length。"Payload Data"="Extension data"+"Application data"，Extension data可能是0
* Masking-key: 0或4字节。所有从客户端发送到服务器的数据都进行了被该字段进行了掩码处理，并且**Mask**设置为1；如果**Mask**设置为0，那么这个值就是0字节。这个值是客户端随机选择的，不能被预测、每帧都是新的值。这个值不影响Payload Data的长度。掩码处理的方式为`payload[i]=payload[i] XOR Masking-key[i MOD 4]`；还原数据的方式为`payload[i] XOR Masking-ey[i MOD 4]`
* **Payload Data**: 包含扩展数据和程序数据的。如果没有使用扩展，扩展数据长度为0。任何扩展必须指定扩展数据的长度，或者长度怎么计算，并且在握手时要协商好扩展如何使用。扩展数据后面的都是程序数据

### 分帧
* 分帧的主要意图是当没必要缓冲消息时，允许发送未知大小的消息。如果消息不分帧，就必须在发送前缓冲整个消息来计算消息长度。使用了分帧，服务器或中间人可能会选择一个合理的缓冲大小，当缓冲满了把一帧写入网络。分帧的第二个用途是多路复用，并不希望一个大消息阻塞了输出
* 如果没有使用扩展，或者使用了扩展但中间人理解了全部的扩展并知道怎么合并/切割帧，那么中间人可能会合并/切割帧，这暗示发送者和接收者不能依赖特定的帧边界
* 分帧的规则如下：
* 一个未分帧的消息由一帧组成，这个帧`FIN=1 && opcode!=0`
* 一个分帧的消息由以下部分组成：一个`FIN=0 &&  opcode!=0`的帧、任意个`FIN=0 && opcode=0`帧、一个`FIN=1 && opcode=0`的帧
* 控制帧可能夹在消息分帧中间，但控制帧绝不能分帧
* 消息分帧必须按顺序发送
* 一个消息分帧绝不能插入到另一个消息分帧中，除非协商了扩展能解释这种情况
* 服务器/客户端必须能处理夹在消息分帧中的控制帧
* 发送者可能创建任意大小的消息分帧
* 客户端和服务器必须同时支持接收分帧和不分帧的消息
* 因为控制帧不能分帧，中间人不能改变控制帧的分帧
* 如果设置了**RSV**位，并且中间人不知道这些位的意思，那么它就不能改变分帧
* 在使用了扩展的连接中，如果中间人不清楚扩展的机制，就不能改变分帧。类似的，如果中间人没有看到websocket握手，就不能改变分帧
* 一个消息的所有分帧都是相同类型，由第一个帧的**opcode**确定。因为控制帧不能分帧，所以一个消息的所有分帧的类型肯定是text、binary或保留的opcode
* 如果控制帧不能夹在消息分帧中，那么遇到一个大消息ping帧的间隔就会很长
* 没有使用扩展的情况下，接收端不需要缓冲一个完整的帧才处理，例如“流”(streaming API)，帧的一部分传送给程序。但是这种方式不一定适用于将来所有的扩展。

### 控制帧
* **opcode**的最高有效位为1。控制帧用于沟通websocket的状态，控制帧的playload length必须小于等于125字节，并且不能分帧

#### Close
* 关闭帧。`opcode=0x8`。关闭帧可能包含一个Application Data，用于标识关闭的理由，例如端点断开、收到的帧太大或收到一个不合法的帧。如果有Application Data，前2个字节必须是无符号整数(网络字节序)，代表状态码（如下）。紧跟着是UTF-8编码的数据作为reason（也可能没有原因），这个原因可能不是给人们看的，但可能用于调试或传递有关的信息。如果是客服端发出的关闭帧，要进行掩码处理。

```
1000 - 正常关闭(Normal Closure)
1001 - 离开(例如服务器关闭、浏览器页面改变)(Going Away)
1002 - 由于协议错误终止连接(Protocol error)
1003 - 接收了错误的数据类型（例如收到的类型标识为text，但数据是binary）(Unsupported Data)
1004 - (保留)(---Reserved----)
1005 - 保留的，不能作为状态码使用。用于标识没有提供状态码(No Status Rcvd)
1006 - 保留的，不能作为状态码使用。标识不正常关闭，例如没有发送或接收一个关闭帧(Abnormal Closure)
1007 - 收到的消息数据不都是由指定类型组成。例如非UTF-8的数据包含了文本信息(Invalid frame payload data)
1008 - 收到的消息违反了策略。这是一个通用的状态码，在没有合适的状态码（例如1003、1009）或需要隐藏特殊细节的情况下使用(Policy Violation)
1009 - 消息太大(Message Too Big)
1010 - 在握手时期望服务器协商一个或多个扩展，但服务器没返回扩展，扩展列表应该放在关闭帧的reason里(Mandatory Ext.)
1011 - 服务器遇到一个意想不到的问题而无法满足请求(Internal Server Error)
1015 - 保留。标识TLS握手失败而关闭连接，例如服务器证书不能被认证(TLS handshake)

保留的状态码
0-999 - 不使用
1000-2999 - 协议使用
3000-3999 - 用于库、框架和应用程序
4000-4999 - 私用
```
* 发出关闭帧后不能再发送数据帧

#### Ping
* `opcode=0x9`，可能包含Application Data，一旦收到Ping帧，必须尽快响应Pong帧，除非收到Close帧。在连接建立后、断开前，在任意时候都可能发送Ping帧，以保持连接或确保另一端始终能响应

#### Pong
* `opcode=0xA`，Pong帧必须返回Ping帧相同的消息
* 如果连续收到多个Ping帧，可能会选择处理最后一个Ping帧
* 自发的Pong帧是不可预期的

### 数据帧
* 数据帧传输应用层and/or扩展层的数据

#### Text
* Payload Data是UTF-8编码。注意，如果是分帧消息，一个分帧可能包含的不是有效的UTF-8编码，但组装起来的消息是UTF-8编码
* 如果接收到的不是UTF-8编码的文本，必须标识连接失败，这个规则也适用于握手阶段

#### Binary
* 这里的数据是任意格式的，完全取决于应用层怎么解释

### 扩展
* Extension Data可能放到Payload Data中，在Application Data之前
* 扩展名称由IANA定义
* 协商好的扩展依次被调用

### 发送数据
* 必须保证连接处于OPEN状态，如果不是就中断发送
* 把数据组织成上面的帧格式，如果数据过大或不可用(???)，就进行分帧
* 第一个帧如果包含数据，**opcode**必须设置成合适的值以便被接受者解释
* 最后一个包含数据的帧的**FIN**设置成1
* 如果协商好了扩展，可能得考虑这些扩展的应用
* 组织好的帧必须通过底层连接传输

### 接收数据
* 接收的数据按照上图的帧格式解释
* 一旦接收了数据帧，必须按照**opcode**定义的格式来解释
* 服务器端必须解码数据
* 如果是分帧消息，需要把所有的帧数据组合起来
* 扩展可能影响数据解析的方式


## Closing the Connection
* 一旦发送或接收到关闭帧，就认为开始了连接关闭过程，就将连接置为CLOSING
* 当一个端收到了关闭帧，必须尽快发送一个关闭帧作为响应，通常把收到的状态码原样返回去。如果收到关闭帧时正在发送一个分帧消息，可以等当前消息发送完再响应关闭帧。响应完关闭帧后，有可能继续处理数据
* 在发送和接收了关闭帧后，就认为连接关闭，并且必须关闭TCP连接。服务器必须立即关闭TCP连接[(没看懂)](https://tools.ietf.org/html/rfc6455#section-7.1.1)；客户端应该等服务器关闭连接，但可能在发送和收到关闭帧后的任意时刻关闭连接，例如在合理的时间范围内没收到服务器的TCP关闭消息
* 如果客户端和服务器同时发送了关闭消息，那么两端收到关闭帧后应该认为websocket连接关闭，然后关闭TCP连接
* 应该使用一个方法明确关闭底层连接，包括TLS会话。
* 如果收到攻击，也可以关闭连接
* 一旦TCP连接关闭，就将连接状态置为关闭
* 在某些异常关闭连接时客户端应该重试恢复连接。比如第一次5s后重连，随后逐渐延长重连等待时间
