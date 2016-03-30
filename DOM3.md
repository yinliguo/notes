## DOM

文档对象模型（Document Object ModelDOM）是为有效的HTML文档和格式良好的XML文档提供的应用程序接口。定义了文档的逻辑结构和被文档被访问和操作的方式。
![模块依赖图](https://raw.githubusercontent.com/yinliguo/notes/master/img/dom-architecture.png)

### 基本接口：Core Modules
- Exception DOMException

> DOM操作抛出的异常

```
DOMSTRING_SIZE_ERR: 2，如果指定的文本不能转换成DOMString
HIERARCHY_RQUEST_ERR: 3，节点插入到了不正确的位置
INDEX_SIZE_ERR: 1，如果index或size是负值，或者大于允许的值
INUSE_ATTRIBUTE_ERR: 10，尝试新增加一个已存在的属性
INVALID_ACCESS_ERR: 15，如果一个参数或一个操作不被基本对象支持
INVALID_CHARACTER_ERR: 5，如果指定一个无效的或不合法的字符，例如在XML的name中
INVALID_MODIFICATION_ERR: 13，如果尝试修改一个基本对象的类型
INVALID_STATE_ERR: 11，使用一个不可用的对象
NAMESPACE_ERR: 14，如果尝试去创建或修改一个错误的命名空间的对象
NOT_FOUND_ERR: 8，如果指向一个不存在的节点
NOT_SUPPORTED_ERR: 9，如果实现不支持对象或操作的请求类型
NO_DATA_ALLOWED_ERR: 6，如果数据被指定给了一个不支持数据的节点
NO_MODIFICATION_ALLOWED_ERR: 7，修改一个不能被修改的对象
SYNTAX_ERR: 12，无效的或不合法的字符串
TYPE_MISMATCH_ERR: 17，如果一个对象的类型和关联这个对象的参数的类型冲突
VALDATION_ERR: 16，如果一个方法（例如insertBefore或removeChild）使节点验证无效
WRONG_DOCUMENT_ERR: 4，如果一个节点在不是创建它的文档中使用
```

- Interface DOMString

> 提供了有序的DOMString值的集合的抽象，没有定义这个集合的实现。

```
属性
length: 这个DOMString的list的长度

方法
contains(DOMString str): 测试一个字符串是否是DOMStringList的一部分
item(unsigned long index): 返回第index个元素。如果参数超出了索引最大值，则返回null
```

- Interface NameList
> 同时提供了name和namespace的有序集合的抽象，没有定义如何实现

```
属性
length: name和namespaceURI的list的长度

方法
contains(DOMString str): 测试一个name是否是NameList的一部分
containsNS(DOMString namespaceURI, DOMString name): 测试namespaceURI/name是否是NameList的一部分
getName(unsigned long index): 返回第index个name元素
getNamespaceURI(index): 返回第index个namespaceURI元素
```

- Interface DOMImplementationList

> 提供了有序的DOM implementation的集合的抽象，但没有定义和约束如何实现

```
属性
length: 集合的长度

方法
item(index): 第index个元素
```

- Interface DOMImplementationSource

> This interface permits a DOM implementer to supply one or more implementations, based upon > > > requested features and versions, as specified in DOM Features. Each implemented > > > > > > > > > DOMImplementationSource object is listed in the binding-specific list of available sources so > > that its DOMImplementation objects are made available.

```
方法
getDOMImplementation(features of type DOMString): 请求支持指定特定的第一个DOM实现
getDOMImpletationList(features of type DOMString): 与上面的方法相同，只不过返回的是一个list列表
```

- Interface DOMImplementation

> 提供了一些独立于文档对象模型的操作

```
方法
createDocument(DOMString namespaceURI, DOMString qualified, DocumentType doctype): 创建一个指定类型的文档对象
createDocumentType(DOMString qualifiedName, DOMString publicId, DOMString systemId): 创建一个空的DocumentType节点
getFeature(DOMString feature, DOMString version): 返回一个实现了指定功能和版本的APIS的对象
hasFeature(DOMString feature, DOMString version): 检测DOM实现是否实现了指定的功能和版本
```

- Interface DocumentFragment

> 是一个“轻量级”的文档对象

- Interface Document

> Document接口代表了整个的HTML或XML文档。概念上来说，它是文档树的根，是访问文档数据的主要权限

```
属性
doctype(DocumentType, readonly): 文档类型声明
documentElement(Element, readonly): 这是一个方便的属性用于访问文档的子节点元素
documentURI(DOMString): 文档的位置
domConfig(DOMConfiguration): 用于Doucment.normalizeDocument()的配置
implementation(DOMImplementation, readonly): 操作文档的DOMImplementation对象。一个DOM程序可能用多种实现的对象
inputEncoding(DOMString, readonly): 指定文档解析时的编码。当未知时，这个属性为null，例如在内存中创建对象
strictErrorChecking(boolean): 指定强制或不强制错误检查
xmlEncoding(DOMString, readonly): XML文档编码
xmlStandalone(boolean): 作为XML文档声明的一部分，指定文档是否是独立的，未指定时为false
xmlVersion(DOMString): 作为XML文档声明的一部分，指定了文档的版本号

方法
adoptNode(Node source): 从别的文档“过继”一个节点到该文档
cerateAttribute(DOMString name): 创建一个名称为name的属性
createAttributeNS(DOMString namespaceURI, DOMString name): 创建一个指定namespaceURI和name的属性
createCDATASection(DOMString data): 创建一个值为data的CDATASection节点
createComment(DOMString data): 创建一个指定字符串的注释节点
createDocumentFragment: 创建一个空的DocumentFragment节点
createElement(DOMString tagName): 创建一个指定类型的节点
createElementNS(DOMString namespaceURI, DOMString qualifiedName): 创建一个指定namespace URI和qualified name的元素
createEntityReference(DOMString name): 创建一个EntityReference对象
createProcessingInstruction(DOMString target, DOMString data): 使用指定的名称和数据字符串创建一个ProcessingInstruction节点
createTextNode(DOMString data)创建一个文本节点
getElementById(DOMString elementId): 返回id为elementId的元素
getElementsByTagName(DOMString tagname): 返回节点名称为tagname的元素组成的NodeList
getElementsByTagNameNS(DOMString namespaceURI, DOMString localName): 返回指定namespaceURI和localName的元素组成的NodeList
importNode(Node importedNode, boolean deep): 从别的文档复制一个节点到该文档。
normalizeDocument: This method acts as if the document was going through a save and load cycle, putting the document in a "normal" form. As a consequence, this method updates the replacement tree of EntityReference nodes and normalizes Text nodes, as defined in the method Node.normalize().
renameNode(Node n, DOMString namespaceURI, DOMString qualifiedName): 重命名元素节点或属性节点的名称
```

- Interface Node

> Node接口是整个文档对象模型的主要数据类型，它代表了文档树种的一个节点。

```
属性
attributes(namedNodeMap, readonly): 该节点的属性
baseURI(DOMString, readonly): 节点的base URI。如果没有实现该属性，则为null
childNodes(NodeList, readonly): 子节点
firstChild(Node, readonly): 第一个子节点，如果没有就为null
lastChild(Node, readonly): 最后一个子节点，如果没有就为null
localName(DOMString, readonly): 节点的qualified name的local part
namespaceURI(DOMString, readonly): 节点的namespaceURI，如果未指定则为null
nextSibling(Node, readonly): 下一个兄弟元素，如果没有返回null
nodeName(DOMString, readonly): 节点的名称见下表
nodeType(unsigned short, readonly): 基本对象类型，如下：
ATTRIBUTE_NODE: 2
CDATA_SECTION_NODE: 4
COMMENT_NODE: 8
DOCUMENT_FRAGEMENT_NODE: 11
DOCUMENT_NODE: 9
DOCUMENT_TYPE_NODE: 10
ELEMENT_NODE: 1
ENTITY_NODE: 6
ENTITY_REFERENCE_NODE: 5
NOTATION_NODE: 12
PROCESSING_INSTRUCTION_NODE: 7
TEXT_NODE: 3
nodeValue(DOMString): 节点的值，依赖节点类型，见下表
ownerDocument(Document, readonly): Document对象，用于创建新节点
parentNode(Document, readonly): 父节点
prefix(DOMString): 节点的命名空间前缀
previousSibling(Node, readonly): 前面的兄弟元素，如果没有返回null
textContent(DOMString): 返回该节点和后代节点的文本内容

方法
appendChild(Node newChild): 增加新的节点到该子节点的最后
cloneNode(boolean deep): 克隆节点，如果deep为true则克隆整个DOM树
copareDocumentPosition(Node other): 比较两个节点的位置关系
getFeature(DOMString feature, DOMString version): 返回实现了指定的功能和版本的APIS的对象（没看懂）
getUserData(DOMString key): 获取关联到该节点的key对应的值
hasAttributes: 该节点是否有属性
hasChildNodes: 该节点是否有子节点
insertBefore(Node newChild, Node refChild): 把newChild插入到子节点refChild之前。如果没有refChild则插入到子节点的最后
isDefaultNamespace(namespaceURI): 检测指定的namespaceURI是否是默认的命名空间
isEqualNode(Node arg): 测试两个节点是否相等
isSameNode(Node other): 测试两个节点是否相同
isSupported(DOMString feature, DOMString version): 测试DOM实现是否支持该节点的feature特性
lookupNameSpaceURI(DOMString prefix): 查找与给出的前缀相关联的namespace URI
lookupPrefix(namespaceURI): 查找与namespace URI相关联的前缀
normalize: （看不懂官方解释）
removeChild(Node oldChild): 删除子节点
replaceChild(Node newChild, Node oldChild): 替换子节点
setUserData(DOMString key, DOMUserData data, UserDataHandler handle): 把键值对关联到节点上
```
![nodeName and nodeValue]({{site.baseurl}}/https://raw.githubusercontent.com/yinliguo/notes/master/img/nodeNameValue.png)

- Interface NodeList

> 提供了有序节点集合的抽象

```
属性
length: 集合长度

方法
item(index): 返回第index元素
```

- Interface NamedNodeMap

> 实现了NamedNodeMap接口的对象用于表示可以通过名称访问节点的集合。注意NamedNodeMap不继承NodeList，元素没有特别的顺序

```
属性
length: 集合长度

方法
getNamedItem(DOMString name): 通过名称查找节点
getNamedItemNS(DOMString namespaceURI, DOMString name): 通过namespaceURI和local name查找节点
item(index): 返回第index个节点
removeNamedItem(name): 通过名称移除节点
removeNamedItemNS(DOMString namespaceURI, DOMString localName): 移除指定namespaceURI和local name的节点
setNamedItem(Node arg): 使用nodeName属性增加一个节点
setNamedItemNS(Node arg): 使用namespaceURI和localName增加一个节点
```

- Interface CharacterData

> 扩展了一套属性和方法用于访问DOM中的字符数据

```
属性
data(DOMString): 节点的字符数据
length: 长度

方法
appendData(DOMString arg): 增加到节点的字符数据后面
deleteData(unsigned long offset, unsigned long count): 移除字符数据
insertData(unsigned long offset, DOMString arg): 插入字符数据
replaceData(unsigned long offset, unsigned long count, DOMString arg): 替换字符数据
substringData(unsigned long offset, unsigned long count): 提取子字符串
```

- Interface Attr

> 代表元素对象的的一个属性，Attr对象继承Node接口，但由于它没有实际的节点，因此它的parentNode、previousSibling等都是null

```
属性
isId(boolean, readonly): 是否是id属性
name(DOMString, readonly): 属性的名称
ownerElement(Element, readonly): 属性所在的元素的节点
schemaTypeInfo(TypeInfo, readonly): 属性的类型信息
specified(boolean, readonly): 如果属性的值是明确给出的，那么返回true
value(DOMString): 当被取得时候，属性的值会被转换成字符串。
```

- Interface Element

> 代表了一个HTML或XML中的元素

```
属性
schemaTypeInfo(TypeInfo, readonly): 元素的类型信息
tagName(DOMString, readonly): 元素的名称

方法
getAttribute(DOMString name): 获取属性的值
getAttributesNS(DOMString namespaceURI, DOMString localName): 通过local name和namespace URI来获取属性值
getAttributeNode(DOMString name): 通过名称获取属性节点
getAttributeNodeNS(DOMString namespaceURI, DOMString localName): 同上
getElementsByTagName(DOMString name): 返回所有标签名称为name的后代元素组成的NodeList
getElementsByTagNameNS(DOMString namespaceURI, DOMString localName): 同上
hasAttribute(DOMString name): 是否有name属性
hasAttributeNS(DOMString namespaceURI, DOMString localName): 同上
removeAttribute(DOMString name): 通过name移除属性
removeAttributeNS(DOMString namespaceURI, DOMString localName): 通过namespace URI和local name移除属性
removeAttributeNode(Attr oldAttr): 移除属性节点
setAttribute(DOMString name, DOMString value): 增加一个新的属性
setAttributeNS(DOMString namespaceURI, DOMString qualifiedName, DOMString value): 同上
setAttributeNode(Attr newAttr): 增加一个新的属性节点
setAttributeNodeNS(Attr newAttr): 增加一个新的属性节点
setIdAttribute(DOMString name, boolean isId): 如果isId是true，则把指定的属性声明为user-determined ID属性
setAttributeNS(DOMString namespaceURI, DOMString localName, boolean isId): 同上
setAttributeNode(Attr idAttr, boolean isId): 如果isId是true，则把指定属性声明为user-determined ID属性
```

- Interface Text

> 继承CharacterData，代表元素或属性的文本内容

```
属性
isElementContentWhitespace(boolean, readonly): 是否包含element content whitespace
wholeText(DOMString, readonly): 返回文本节点的文本

方法
repalceWholeText(DOMString content): 替换当前节点和所有相邻文本节点的文本
splitText(unsigned long offset): 把该节点分割成两个节点
```

- Interface Comment

> 继承CharacterData，代表注释的内容

- Interface TypeInfo

> 代表了一个元素节点或属性节点的类型。

```
属性
typeName(DOMString, readonly): 类型名称
typeNamespace(DOMString): 类型的命名空间

方法
isDerivedForm(DOMString typeNamespaceArg, DOMString typeNameArg, unsigned long derivationMethod): This method returns if there is a derivation between the reference type definition
```

- Interface UserDataHandler

> 节点对象操作的接口

```
方法
handle(unsigned short operation, DOMString key, DOMUserData data, Node src, Node dst): 用于节点被导出或克隆时调用
```

- Interface DOMError

> 描述错误的接口

```
属性
location(DOMLocator, readonly): 错误的位置
message(DOMString, readonly): 描述错误信息的字符串
relateData(DOMObject, readonly): 相关的DOMError.type依赖数据
severity(unsigned short, readonly): 严重程度。可选值有SEVERITY_WARNING, SEVERITY_ERROR, SEVERITY_FATAL_ERROR
type(DOMString, readonly): 标志哪种类型的related data是relatedData期待的
```

- Interface DOMErrorHandler

> 这是一个发生错误时的回调函数接口

```
方法
handleError(DOMError error): 当错误发生时调用该函数
```

- Interface DOMLocator

> 描述位置的接口

```
byteOffset(long, readonly): 输入源的字符偏移
columnNumber(long, readonly): locator指向的列数
lineNumber(long, readonly): locator指向的行数
relatedNode(Node, readonly): locator指向的节点
uri(DOMString, readonly): locator指向的URI
utf16Offset(long, readonly): 
```

- Interface DOMConfiguration

> The DOMConfiguration interface represents the configuration of a document and maintains a table of recognized parameters

```
属性
parameterNames(DOMStringList, readonly): DOMConfiguration对象支持的参数列表，至少有一个值可以被修改

方法
canSetParameter(DOMString name, DOMUserData value): 检测是否支持设置一个参数的值
getParameter(DOMString name): 获取参数的值
setParameter(DOMString name, DOMUserData value): 设置参数的值
```