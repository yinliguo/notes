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
- If the top and bottom margins of an element with clearance are adjoining, its margins collapse with the adjoining margins of following siblings but that resulting margin does not collapse with the bottom margin of the parent block.(没看懂)

两个margin相邻必须满足以下条件：
- 都属于同一个block formatting context的block-level boxes
- 没有line boxes、clearance、padding或border分开它们
- 都属于垂直相邻的box边界，例如：
	- 一个box的top margin和它的第一个in-flow子元素的top margin
    - 一个box的bottom margin和它的下一个in-flow兄弟元素的top margin
    - 最后一个in-flow子元素的bottom margin和它的父元素的bottom margin并且这个父元素的height是auto
    - 一个box没有创建新的block formatting context、它的min-height是0，height是0或auto并且没有in-flow子元素，它的top margin和bottom margin
    
> 如果collapsed margin的任何一个component margin与另一个margin相邻，就认为这个collapsed margin和这个margin相邻

上面的规则暗示：
- 在floated box和其它任意box之间的margins不会collapse（甚至floated box和它的第一个in-flow子元素也不会）
- 创建了一个新的block formatting context的元素的margins不会collapse（例如floats和overflow属性不是visible的元素）
- aboslutely positioned boxes的margins不会collapse（甚至和它们的第一个in-flow子元素也不会）
- inline-block boxes的margins不会collapse（甚至和它们的第一个in-flow子元素也不会）
- in-flow block-level元素的bottom margin总是和它下一个in-flow block-level兄弟元素的top margin collapse，除非这个兄弟元素有clear属性（即时加了clear这个在chrome和firefox上仍然collapse）
- 如果一个in-flow block元素没有top border和top padding，并且它的第一个in-flow子元素没有clearance，那么它的top margin和子元素的top margin会collapse
- 如果一个in-flow block元素没有bottom padding和bottom border、height属性为auto、min-height为0，并且它的最后一个in-flow block-level子元素的bottom margin没有和带有clearance的top margin collapse，那么它的bottom margin和这个子元素的bottom margin会collapse
- 如果一个box的min-height为0、没有top border/bottom border/top padding/bottom padding、height为0或auto、不包含一个line box并且它所有的in-flow子元素的margins都collapse，那么它自己的margins会collapse（在chrome和firefox中也无效）

> 当有两个甚至更多的margins进行collapse时，结果是最大的margin width。如果有负值margin，就让（最大的正margin值） -（最大的负margin值的绝对值）。如果没有正margin值，就是0 - （最大负margin值的绝对值）

> If the top and bottom margins of a box are adjoining, then it is possible for margins to collapse through it. In this case, the position of the element depends on its relationship with the other elements whose margins are being collapsed.（没看懂）

### Padding属性
padding属性是box的padding area的宽度，是4条边的缩写属性。
- 值可以为固定值或百分比，百分比是相对于containing block
- 初始值为0
- 非继承属性

### Border属性
padding属性是box的padding area的宽度、颜色和样式，是4条边的缩写属性。

border-width
- 值可以为固定值
- 初始值为medium
- 非继承属性

border-color
- 值为color|transparent|inherit
- 非继承属性

border-style
- 值为border-style|inherit
- 非继承属性

![](https://raw.githubusercontent.com/yinliguo/notes/master/img/5AAyW.png)
