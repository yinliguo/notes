## Box model

> The CSS box model describes the rectangular boxes that are generated for elements in the document tree and laid out according to the visual formatting model.（盒模型就是在文档树中为元素生成的、按照可视化格式模型布局的矩形盒子）

### Box dimensions（盒子尺寸）
每一个box都有一个content area和可选的padding、border、margin。如下图所示：
![box尺寸](https://raw.githubusercontent.com/yinliguo/notes/master/img/boxdim.png)

content area的尺寸依赖以下几个因素，具体计算方式在visual formatting model中讨论
- 元素生成的box是否设置了width和height属性
- box是否包含text或其他的box
- box是否是table
- ......

content、padding和border的背景由background属性指定，margin的背景是透明的

### margin属性
margin属性是box的margin area的宽度，是4条边的缩写属性。
- 值可以为固定值或百分比，百分比是相对于containing block
- 初始值为0
- 非继承属性

##### Collapsing margin（外边距塌陷）
> 在CSS中，两个或多个box（不一定是兄弟元素）的相邻的margins会结合成一个margin。margins通过这种方式结合叫做collapse（塌陷），最终得到的结合的margin叫做collapsed margin。

注意：水平的margin不会collapse，只有相邻的垂直的margin才会collapse，除了以下两种情况
- 根元素（html）的margin不会collapse
- 


