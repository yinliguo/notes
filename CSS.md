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
:nth-of-type(x)：x取值同上，取同类型元素
:nth-last-of-type(x)：x取值同上，取的顺序与上面相反
:first-child：同nth-child(1)
:last-child：同nth-last-child(1)
:first-of-type：同nth-of-type(1)
:last-of-type：同nth-last-of-type(1)
:only-child：父元素只有一个子元素，匹配子元素
:only-of-type：只包含一个同类型的子元素，匹配子元素
:empty：没有子元素也没有内容的元素
:not()：去掉括号中的元素
```

- 伪元素
```
::first-line：一个元素第一行内容
::first-letter：第一个字母
::before：之前的内容
::after：之后的内容
::selection：用于选择文本时的样式，只能定义background和color
```

- 层次选择器
```
空格：后代
>：子选择器
+：相邻的、后面的兄弟元素
~：后面的兄弟元素
```
> 选择器权重计算方式  
> 1. id选择器的数量=a  
> 2. class、属性选择器和伪类选择器的数量=b  
> 3. 类型选择器和伪元素选择器的数量=c  
> 4. 忽略通用选择器  
> 5. 权重 = a * 100 + b * 10 + c  

### 盒模型
以快递包裹为例：
- content：内容（预订的东西）
- padding：内边距（易碎的东西会有一层防震气泡膜填充物）
- border：边框（包裹的外包装纸箱）
- margin：外边距（与其他包裹的距离）

### 布局
- margin、padding
- float
- position、top、left、right、bottom
- flex
- columns

### 样式
- 颜色
> 颜色模式有：RGB、RGBA、HSL、HSLA 
> 透明度属性opacity

- 文字
```
font-family：字体
font-size：字号
font-weight：字体粗细
font-style：字体风格，比如斜体
```
- 背景
```
background-color：背景颜色
background-image：背景图片
background-repeat：重复方式
background-attachment：背景是否固定
background-position：背景图片位置
background-clip：背景图片的显示范围，是否包含padding或border
background-origin：背景图片的起点，是相对于content、padding还是border的左上角
background-size：背景图片大小
```
- 边框
```
```
- 阴影
```
box-shadow：
text-shadow：
```
- 动画
```
```
- 媒体查询
```
```