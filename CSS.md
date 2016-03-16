## CSS
> CSS（层叠样式表）是对HTML文档进行排版和样式调整的规范

### 选择器
- \*：通用选择器
- 类型选择器：span、div
- id：每个页面的id不能重复
- class：样式选择器，如果有多个元素用到相同样式，可以定义一个class
- 属性选择器
```
[attr]含有attr属性的元素
[attr=val]属性attr为val的元素
[attr~=val]属性attr的值中包含val，并且是以空格分割
[attr|=val]属性attr的值以val或val-开头
[attr^=val]属性attr的值以val开头
[attr$=val]属性attr的值以val结尾
[attr*=val]属性attr的值包含val
```
- 伪类选择器
```
// 动态伪类样式
:link：用于还没有被访问过的链接
:visited：已经被用户访问过的链接

// 用户动作伪类样式
:hover：鼠标经过
:active：目标激活，例如被点击时
:focus：获得焦点，接受键盘或鼠标事件

// 目标伪类样式
:target：设置被点击链接的锚点的样式

// 语言伪类样式
:lang()：设置不同语言下文档的样式，一般用于多语言的网站

// UI元素状态伪类，常用于表单中的元素
:enabled：启用元素
:disabled：禁用元素
:checked：选中状态，用于单选或复选框
:indeterminate：不确定的状态，用于单选或复选框

// 结构伪类
:root：根元素，即<html>
:nth-child(x)：x取值可以为an+b(n从1开始)、odd（奇数）、even（偶数）
:nth-last-child(x)：x取值同上，但是从最后一个元素开始
```

### 盒模型
以快递包裹为例：
- content：内容（预订的东西）
- padding：内边距（易碎的东西会有一层防震气泡膜填充物）
- border：边框（包裹的外包装纸箱）
- margin：外边距（与其他包裹的距离）

### 