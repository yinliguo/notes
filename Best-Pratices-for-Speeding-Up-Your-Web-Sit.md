## Best-Pratices-for-Speeding-Up-Your-Web-Sit
> 翻译自[雅虎性能提升规范](https://developer.yahoo.com/performance/rules.html)

### Content
#### 1、Minimize HTTP Requsts（减少http请求）
* 合并文件。例如合并css、js文件
* 雪碧图
* 图片地图
* 内联图片

#### 2、Reduce DNS Lookups（减少DNS查询）
* 减少域名数量，但考虑到并行下载，建议保留2～4个域名

#### 3、Avoid Redirects（避免重定向）
#### 4、Make Ajax Cacheable（缓存Ajax）
#### 5、Post-load Components（推迟加载组件）
#### 6、Preload Components（预加载组件）
#### 7、Reduce the Number of DOM Elements（减少DOM元素数量）
#### 8、Split Components Across Domains（把组件分配到不同的域名）
> 为了提高并行下载的数量

#### 9、Minimize the Number of iframes（减少iframe数量）
> iframe 会延迟onload事件

#### 10、No 404s（避免404）

### Server
#### 11、Use a Content Delivery Netowrk（使用CDN）
#### 12、Add an Expires or a Cache-Control Header（增加使用过期时间或Cache-Control header）
* 对于静态资源通过增加`Expires`来实现永不过期
* 对于动态资源使用合适的`Cache-Control`

#### 13、Gzip Components（使用Gzip）
#### 14、Configure ETags（配置ETag）
* 响应中的`ETag`代表资源的标志，下次发请求时会在`If-None-Match`携带，如果没有改变，就返回304，否则返回新的资源
* ETag与服务器绑定，如果换了服务器，ETag就变了
* 如果不使用ETag就应该把它关闭，减少数据传输

#### 15、Flush the Buffer Early（尽早输出缓冲）
#### 16、Use GET for AJAX Requests（尽量使用GET请求）
* Yahoo! Mail team发现当使用XMLHttpRequest，POST请求会分2步处理，先发送headers，再发送数据

#### 17、Avoid Empty Image src（避免图片地址为空）
* 原文解释说会发请求，但试验了chrome和safari都没有发，可能是浏览器已经处理了这个问题

### Cookie
#### 18、Reduce Cookie Size（减小cookie）
* 去掉没必要的cookie
* 设置合适的域名和过期时间

#### 19、Use Cookie-free Domains for Components
* 静态资源的域名不要跟网站域名相同，以减少携带没用的cookie

### CSS
#### 20、Put Stylesheets	at the Top（把样式放在顶部）
#### 21、Avoid CSS Expressions（避免CSS表达式）
#### 22、Choose <link> over @import（使用<link>而不是@import）
* 在IE里@import相当于把样式放到底部

#### 23、Avoid Filters（避免使用Filters）
* `AlphaImageLoader `是为了修复IE7以下的浏览器的png图片颜色问题，在图片下载过程中会阻塞浏览器渲染并冻结浏览器

### JavaScript
#### 24、Put Scripts at the Bottom（把脚本放到底部）
#### 25、Make JavaScript and CSS External（把js和css导出成文件）
#### 26、Minify JavaScript and CSS（压缩js和css）
#### 27、Remove Duplicate Scripts（移除重复的脚本）
#### 28、Minimize DOM Access（减少DOM访问）
* 缓存元素的引用
* 离线更新节点，然后添加到DOM树
* 避免使用js调整布局

#### 29、Develop Smart Event Handlers（开发智能的事件处理）

### Images
#### 30、Optimize Images（优化图片）
#### 31、Optimize CSS Sprites（优化css雪碧图）
#### 32、Do Not Scale Images in HTML（不要在HTML中缩放图片）
#### 33、Make favicon.ico Small and Cacheable（使favicon.ico尽量小并且可以缓存）

### Mobile
#### 34、Keep Components Under 25 KB（保持组件不大于25kb）
#### 35、	Pack Components into a Multipart Document
* Packing components into a multipart document is like an email with attachments, it helps you fetch several components with one HTTP request (remember: HTTP requests are expensive). When you use this technique, first check if the user agent supports it (iPhone does not).