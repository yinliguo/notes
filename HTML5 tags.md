# HTML5标签

### 全局属性
- accesskey：定义快捷键，激活或聚焦到元素
- class：样式
- contenteditable: 将其变为可编辑输入框
- dir：规定元素内容的文本方向
- hidden：是否隐藏
- id：标识符
- lang：指定元素内容的主要语言
- spellchek：检查拼写和语法
- style：样式
- tabindex：指定按tab键选中的顺序
- title：元素的信息
- translate：是否翻译该元素的文本内容 

### 标签
#### The root element
- html  
文档根元素

#### Document metadata
- head  
文档元数据的集合

- title  
文档的标题和名字，在head中只有一个title，用于历史记录、标签或搜索结果

- base  
文档的基地址，用于解析相对url
> href：文档基地址
> target：Default browsing context for hyperlink navigation and form submission

- link
链接其他的资源  
> href：超链接的地址
> crossorigin：如何操作跨域请求
> rel：包含超链接的文档和目标资源的关系
> media：Applicable Media
> hreflang：链接资源的语言
> type：资源的类型
> sizes：icons的大小（对于rel="icon"）

- meta
表示各种各样的不能使用title、base、link、style、和script标签表示的元数据  
> name：元数据的名称
> http-equiv：Pragma directive
> content：元素的值
> charset：编码
name、http-equiv和charset必须指定一个，一个文档中只能有一个charset，下面是标准的元数据名称
> application-name：应用程序的名称，一个文档只能有一个
> author：作者名称
> description：页面描述，一个页面只能有一个
> generator：生成文档的软件名称
> keywords：页面的关键词，由逗号分隔

- style
内嵌在文档中的样式
> media：Applicable media
> type：内嵌资源的类型

#### Sections
- body
文档内容
> onafterprint：用于打印时执行的js，IE和firefox支持，不过IE中是在打印对话框出现之前执行，firefox是在之后执行
> onbeforeprint：参考onafterprint
> onbeforeunload：在刷新或关闭页面时执行
> onhashchange：当url的锚部分发生变化时执行
> onmessage：消息被触发时执行
> onoffline：浏览器离线时执行
> ononline：浏览器开始在线时执行
> onpagehide：离开页面时执行，比如点击一个连接、刷新页面、提交表单、关闭浏览器等
> onpageshow：加载页面时执行
> onpopstate：每当处于激活状态的历史记录条目发生变化时会执行
> onstorage：当本地存储发生变化时执行
> onunload：退出页面时执行

- article
完整的、独立的模块，例如一篇文章，一条评论，一个交互的组件或者一个独立的条目

- section
表示一个普通的块

- nav
带导航链接的块

- aside
跟内容无关的部分

- h1,h2,h3,h4,h5,h6
标题，不能用于副标题、转换标题、小标题、广告语

- header
介绍的内容，典型的header是头部包含的介绍和广告

- footer
最近的父块级元素的尾部，典型的footer包含关于块的信息，例如谁写的、版权信息、链接到其他文档和收藏等。

- address
最近的article或body的联系信息

#### Grouping content
- p
段落

- hr
段落级的主题中断，例如一个故事的场景切换、过渡到另一个主题

- pre
原格式输出

- blockquote
注释、引用
> cite：链接到注释的出处

- ol
有序的列表
> reversed：反序
> start：第一条的序号
> type：规定在列表中使用的标记类型？

- ul
无序的列表

- li
列表的条目
> value：序号，当父元素是ol时有效

- dl
包含name-value的列表，dt表示name，dd表示value

- dt
条目或名称

- dl
描述、定义或值

- figure
表示流内容

- figcaption
figure内容的标题

- div
没有特殊的含义

- main
文档的主要内容

#### Text-level semantic
- a
如果有href属性表示一个超链接，否则表示一个占位符
> href：超链接地址
> target：打开超链接或提交表单的方式
> download：是否下载该资源取代打开
> rel：包含该超链接的文档和目标资源的关系
> hreflang：链接资源的语言
> type：资源的类型

- em
表示强调

- strong
重要的、严重的、紧急的内容

- small
小号字体

- s
表示不准确或不相关的内容

- cite
对参考文献的引用

- q
引用

- dfn
对特殊术语或短语的定义

- abbr
简称或缩写

- data
机器可读的内容
> value：机器可读的值

- time
标签定义的日期或时间
> datetime：定义日期或时间，如果未定义，则必须在内容中定义

- code
代码片段

- var
变量的名称，或者由用户提供的值，常与<code>或<pre>一起使用表示代码

- samp
程序或系统的输出

- kbd
用户输入，典型的是键盘输入，也可以用于其他的输入，如声音输入

- sup
上标

- sub
下标

- i
斜体。a span of text in an alternate voice or mood, or otherwise offset from the normal prose in a manner indicating a different quality of text, such as a taxonomic designation, a technical term, an idiomatic phrase from another language, transliteration, a thought, or a ship name in Western texts.

- b
粗体。a span of text to which attention is being drawn for utilitarian purposes without conveying any extra importance and with no implication of an alternate voice or mood, such as key words in a document abstract, product names in a review, actionable words in interactive text-driven software, or an article lede.

- u
下划线。a span of text with an unarticulated, though explicitly rendered, non-textual annotation, such as labeling the text as being a proper name in Chinese text (a Chinese proper name mark), or labeling the text as being misspelt.

- mark
高亮

- ruby
定义ruby注释（中文注音或字符），在东南亚使用，显示的是东亚字符的发音

- rb
标记ruby注释的基文本组件

- rt
标记ruby文字组件

- rtc
标记ruby文字容器

- rp
定义不支持ruby元素的浏览器显示的内容

- bdi
脱离父元素的文本方向设置

- bdo
覆盖默认的文本方向
> dir：定义文字的方向

- span
没有任何含义

- br
回车

- wbr
在合适的地方换行

#### Edits
- ins
定义已被插入文档的文本
> cite：引文的链接或关于编辑的更多信息
> datetime：编辑的日期和时间

- del
定义移除的部分
> 属性同ins

#### Embedded content
- img
图片
> alt：图片不可用时的替换文字
> src：图片的地址
> crossorigin：怎样处理跨域请求
> usemap：将图片定义为客户端图像映射，图像映射是带有可点击区域的图像
> ismap：图片是否是一个服务器端图像映射
> width：水平尺寸
> height：垂直尺寸

- iframe
嵌套的浏览器内容
> src：资源的地址
> srcdoc：html文本
> name：名称
> sandbox：沙箱模式，可选值""、allow-same-origin、allow-top-navigation、allow-forms、allow-scripts
> width：水平宽度
> height：竖直高度

- embed
定义嵌入的内容，如插件
> src：资源的地址
> type：内嵌资源的类型
> with：水平宽度
> height：竖直高度

- object
嵌入对象，比如图像、音频、视频、Java applets、ActiveX、PDF和flash
> data：资源的地址
> type：嵌入资源的类型
> typemustmatch：是否type属性和Content-Type值需要匹配要用的资源
> name：嵌套资源的名称
> usemap：图像映射的名称
> form：规定对象所属的一个或多个表单
> width：水平尺寸
> height：竖直尺寸

- param
为object或applet提供参数
> name：参数的名称
> value：参数的值

- video
播放视频、电影或带文字的音频
> src：资源的地址
> crossorigin：如何处理跨域
> poster：播放之前显示的图片
> preload：缓冲的大小
> autoplay：是否自动播放
> mediagroup：视频所属媒体组合的名称，媒体组合允许两个或多个音频、视频元素保持同步
> loop：是否循环播放
> muted：是否默认静音
> controls：显示控制栏
> width：水平宽度
> height：竖直高度

- audio
音频
> 属性同video

- source
定义任意类型的媒体资源
> src：资源地址
> type：资源类型

- track
为video等媒介规定外部文本轨道

- map
定义图像映射，与img一起使用
> name：名称，供usemap属性使用

- area
图像映射上的一个区域
> alt：当图片不可用时的替换文字
> coords：在图像映射上的坐标
> download：是否下载，而不是打开链接
> href：超链接
> hreflang：链接资源的语言
> rel：包含超链接的文档和目标资源的关系
> shape：在图像映射上的形状
> target：打开超链接的方式
> type：引用资源的类型

- math
数学公式

- svg
矢量图片

#### Links
- a
链接

#### Tabular data
- table
表格
> border：边框
> sortable：启用排序接口

- caption
表格的标题

- colgroup
表格的一列或多列的集合
> span：元素的列数

- col
如果它的父元素是colgroup，colgroup的父元素是table，那么它就表示一列或多列
> span：列数

- tbody
行块

- thead
行块的标题

- tfoot
行块的底部

- tr
行

- td
单元格
> colspan：横跨的列数
> rowspan：横跨的行数
> headers：定义表头与单元格的关系，屏幕阅读器会利用该属性

- th
单元格的头部
> 同td
> scope：定义表头数据与单元格关联的方法
> abbr：规定单元格内容的缩写
> sorted：列排列方向

#### Forms
- form
表单
> accept-charset：表单的字符编码
> action：form提交的url
> autocomplete：是否启用表单的自动完成功能
> enctype：发送表单之前如何对数据编码，可选值有application/x-www-form-urlencoded、multipart/form-data、text/plain
> method：表单提交方法，可选值为get、post
> name：表单名称
> novalidate：提交表单时不进行验证
> target：打开action的方式

- label
input元素的定义标签
> form：规定label字段所属的一个或多个表单
> for：绑定的表单元素

- input
数据输入域
> accept：在文件上传时的文件类型
> alt：当图片不可用时的替换文本
> autocomplete：是否自动填充
> autofocus：页面载入时是否自动获取焦点
> checked：首次加载页面时，是否被选中
> dirname：Name of form field to use for sending the element's directionality in form submission
> disabled：是否可用
> form：规定所属的一个或多个表单
> formaction：覆盖表单的action，适用于type="submit"和type="image"
> formenctype：覆盖表单的enctype，适用type="submit"和type="image"
> formmethod：覆盖表单的method，适用于type="submit"和type="image"
> formnovalidate：覆盖表单的novalidate
> formtarget：覆盖表单的target，type="submit"和type="image"
> width：水平尺寸
> height：竖直尺寸
> inputmode：输入方式
> list：引用包含输入字段预定义选项的datalist
> max：最大值
> min：最小值
> maxlength：最大长度
> minlength：最小长度
> multiple：允许多个值
> name：名称
> pattern：输入值的格式
> placeholder：默认值
> readonly：是否可以被编辑
> required：是否必须
> size：字段的宽度
> src：资源的地址
> step：规定输入字的合法数字间隔
> type：元素类型，可选值有hidden、text、search、tel、url、email、password、date、time、number、range、color、checkbox、radio、file、submit、image、reset、button
> value：元素的值

- button
按钮
> autofocus：自动获取焦点
> disabled：是否可用
> form、formaction、formenctype、formmethod、formnovalidate、formtarget同input
> menu：Specifies the element's designated pop-up menu
> name：名称
> type：类型，可选值有button、reset、submit
> value：元素的值

- select
下拉选择框
> autofocus、disabled、form、mutiple、required、size同input

- datalist
一组可选的值，与input配合使用，用自己的id属性充当input的list属性

- optgroup
把相关的选项组合在一起
> disabled：是否可用
> label：为组规定描述

- option
可选值
> disabled：是否可选
> label：描述
> selected：是否被选中
> value：值

- textarea
文本域
> autocomplete、autofocus、dirname、disabled、form、inputmode、maxlength、minlength、name、placeholder、readonly、required同input
> cols：每一行最多有多少个字符
> rows：显示多少列
> wrap：表单提交时，文本域内的文本如何换行

- keygen
用于表单的密钥对生成器字段，当提交表单时，私钥存在本地，公钥发送到服务器
> autofocus：自动获得焦点
> challenge：如果使用，则将keygen的值设置为在提交时询问
> disabled：禁用keytag字段
> form：属于的表单
> keytype：生成密钥的类型
> name：名称

- output
输入
> for：定义输出域一个或多个相关的元素
> form：从属的表单
> name：名称

- progress
进度条
> value：当前值
> max：最大值

- meter
度量衡，仅用于已知最大值和最小值的度量
> value：当前值
> min：最小值
> max：最大值
> low：低值
> high：高值
> optimum：定义什么样的值是最佳

- fieldset
表单内的元素分组
> disabled：是否可用
> form：从属的表单
> name：名称

- legend
fieldset的标题

#### Scripting
- script
脚本
> src：资源地址
> type：资源类型
> charset：脚本资源的编码
> async：是否异步执行
> defer：脚本不会生成任何文档内容，浏览器可以继续解析并绘制页面
> crossorigin：如何跨域

- noscript
脚本未被执行时的替代内容

- template
声明HTML片段，用于脚本克隆并插入到文本中

- canvas
画布
> width：宽度
> height：高度
