## Commonjs

Commonjs是一种规范，标志着如何写模块以便在同一类模块系统（可能是客户端也可能是服务器端、安全的或不安全的、目前支持的或带语法扩展的将来的系统）中相互协作。这些模块被提供顶级域的privacy、从其它模块导入单个对象，并且导出它们自己的API。这暗示着这个规范定义了一个模块系统的最少特点来支持它与其它模块互操作。（以上翻译自[Modules/1.1](http://wiki.commonjs.org/wiki/Modules/1.1.1)）

#### require
require是一个函数，使用规则如下：
- 接收一个模块标志作为参数
- 返回导入模块的API
- 如果有循环依赖的情况，比如A依赖B，那么当A执行到require（B）时，应该能导出B模块中在require（A）之前的代码的exports
- 如果require不能返回模块，就必须抛出错误
- require函数可能有一个"main"属性
	- 如果有可能，这个属性是只读的，不能被删除
    - main属性要么是undefined，要么是一个已加载的模块中表示一个模块
- require方法可能有一个"paths"属性，是一个由路径字符串组成的优先的数组。相对于顶级模块目录从高到低排列。
	- paths属性不能存在于沙盒（一个安全的模块系统）中
    - paths属性在所有模块中必须指定同一个位置
    - Replacing the "paths" object with an alternate object may have no effect.
    - 如果paths属性存在，就地修改paths必须能通过对应模块的搜索行为反映出来
    - If the "paths" attribute exists, it may not be an exhaustive list of search paths, as the loader may internally look in other locations before or after the mentioned paths.
    - 如果paths属性存在，加载器会首先解析并调用路径提供的模块

#### Module Context
- 在一个模块中，存在一个require函数（上面定义的）
- 在一个模块中，存在一个变量exports，这是一个对象，模块可能会添加API到它里面
	- 模块必须使用exports作为导出的对象
- 在一个模块中，必须有一个叫module的对象，
	- 这个module对象必须有一个'id'属性，当require(module.id)时能够返回exports对象（也就是说module.id能传递给其它模块，通过require引入原始的module）。如果可能，这个属性应该是只读的不能被删除
    - 当模块对象被创建时，可能有一个uri字符串标志该模块。uri不能存在沙盒中

#### Module Identifiers
- 模块标志是被'/'分割的条目组成的字符串
- 一个条目必须是驼峰格式、'.'或'..'
- 模块标志可能没有文件扩展名，比如'.js'
- 模块标志可能是相对路径或是顶级路径。如果第一个条目是'.'或'..'，那么就是相对路径
- 顶级标志成概念的模块根命名空间
- 相对路径标识符被解析时是相对于require被调用的模块

#### Unspecified
- Whether modules are stored with a database, file system, or factory functions, or are interchangeable with link libraries.
- 当解析模块标志时，PATH是否被模块加载器支持

-------------
## AMD
Asynchronous Module Definition(AMD)指定了一种机制：模块和它依赖的模块能被异步加载。特别适合浏览器环境，因为浏览器环境下，同步加载会带来性能问题、可用性、调试和跨域访问的问题。

#### define函数
该规范指定了一个define函数，作为一个自由变量或全局变量使用。格式如下
```
define(id?, dependencies?, factory)
```
###### id
模块的id，是字符串类型，顶级的或绝对路径id，可选的。如果没有指定，默认是加载器请求的脚本的模块id。模块id用于标识已经定义的模块，也能用于依赖参数中。在AMD中的模块id是CommonJS中的模块id的超集。

###### dependencies
是被定义模块依赖的数组，依赖必须在factory执行之前被解析，解析的值传递给factory对应位置的参数。

依赖可能是相对id，应该相对于正在定义模块id被解析。这个规范定义了3个特殊的依赖名称，'require'、'exports'、'module'，这些依赖应按照CommonJS规范被解析成自由变量。

依赖是可选的，如果没有定义，则默认是['require', 'exports', 'module']，但是，如果factory的参数小于3，那么加载器会按照factory参数去加载对应的模块

###### factory
是一个函数，用于实例化一个模块或对象，如果是一个函数，那么它只能被执行一次；如果是一个对象，那么就把这个对象分配给模块的exports属性。如果这个factory函数返回一个值（对象、函数或任何为true的值），那么这个值应该作为该模块导出的值。

#### 简化CommonJS封装
如果没有指定依赖参数，模块加载器会选择读取factory函数源代码，并找出require(module-id)的语句，第一个参数被定为这个模块的依赖。

在某些情况下，例如由于代码大小限制或缺少toString函数的支持（Opera没有toString），模块加载器不会去读取factory源代码。

如果依赖参数给定，那么就不应该读取factory的源代码。