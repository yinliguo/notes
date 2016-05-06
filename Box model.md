## Box model

> The CSS box model describes the rectangular boxes that are generated for elements in the document tree and laid out according to the visual formatting model.（盒模型就是在文档树中为元素生成的、按照可视化格式模型布局的矩形盒子）

### Box dimensions（盒子尺寸）
每一个box都有一个content area和可选的padding、border、margin。如下图所示：
![box尺寸](https://raw.githubusercontent.com/yinliguo/notes/master/img/boxdim.png)

content area的尺寸依赖以下几个因素：
1. 元素生成的box是否设置了width和height属性
2. box是否包含text或其他的box
3. box是否是table
4. 其它。
具体计算方式在visual formatting model中讨论

