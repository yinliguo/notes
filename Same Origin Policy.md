## Same Origin Policy

源由URL中的scheme、host和port决定。一般来说，从不同的源获取的文档是互相独立的。例如，从http://example.com/doc.html 获取的文档尝试访问从 https://example.com/target.html 获取的文档，用户代理会禁止访问，因为第一个文档的源（http、example.com、80）不同于第二个文档的源（https、example.com、443）。尽管same-origin policy在APIs之间是有区别的，但首要的意图是当用户访问不受信任的网站时，这些网站不会妨碍用户和受信任的网站之间的session。

对于网络APIs，同源策略区分发送信息和接收信息。大体来说，允许一个源发送信息给另一个源，但不允许一个源从另一个源接收信息。之所以阻止接收信息是为了恶意网站从其它网站读取机密信息，但也阻止了来自其它合法网站提供的内容。

在同源策略下，发送跨域信息也是很危险的，因为它允许了网站攻击，例如cross-site request forgery（CSRF）和clickjacking。The same-origin policy cannot address these security vulnerabilities in the same way it does those around receiving of information since prohibiting cross-site sending of information would prohibit cross-site hyperlinks. （同源策略不能以同一方式区分接收信息的安全与否，因为阻止了信息的跨域发送就会阻止跨域超链接）。不允许发送跨域信息，就意味着没有web了，因为每个源只能链接到它自己。

因为同源策略在发送和接收信息的类web特征上创建（想要创建）一个全面禁令，它可能不是很适合web的权限控制需求。尽管如此，同源策略已经应用到web，并且很多web资源都依赖它。

同源策略限制了网络消息从一个源发送到另一个源。例如，同源策略允许GET和POST，但阻止PUT和DELETE请求。除此之外，源可以发送自定义的HTTP headers给自己，但不能发送自定义的headers给其它的源。

读取其它的源的信息的限制是subtle的。script标签能够执行从其它的源获取的内容，这意味着网站不能依赖于同源策略去保护信息（作为script格式解析，例如，js源文件、JSON格式、JSONP服务、JavaScript/gif polyglots）的机密性。对于这类资源，通过在所有响应中增加Access-Control-Allow-Origin:\*头信息来保护安全性，并且增加了很大的灵活性。



