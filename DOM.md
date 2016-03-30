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