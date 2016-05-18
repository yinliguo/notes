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