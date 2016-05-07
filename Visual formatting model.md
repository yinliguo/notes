## Visual formatting model

visual formatting model：how user agents process the document tree for visual media.（用户代理如何为可视化媒体处理文档树）


在visual formatting model中，文档树中的每个元素按照box model生成0个或多个box，这些box的布局由以下因素影响
- box的尺寸和类型
- positioning scheme（normal flow、float、absolute positioning）
- 文档树的元素之间的关系
- 其它，比如viewport size、images固有的尺寸等

visual formatting model没有定义所有方面（比如字符间距算法），不同的用户代理可能会有不同的实现方式

#### The viewport
continuous media（braille、screen、speech、tty都属于该类型媒体）的用户代理通常提供一个viewport属性，当viewport改变时，用户代理可能会改变文档的布局。如果viewport比文档渲染的画布区域小时，用户代理应该提供一个滚动条。每个画布最多有一个viewport，但是用户代理可能渲染多个画布（例如，提供一个文档的不同views）

#### Containing blocks
很多box的位置和尺寸都是相对于一个叫containing block的矩形box计算的。一般而言，生成的box为它的子元素扮演着containing block的角色。每一个box的位置都是相对于containing block给出，但它并不局限于containing block，它可能会overflow（溢出）

#### Controlling box generation（控制box的生成）
##### Block-level elements和block boxes
block-level elements：源码文档中作为blocks被格式化的元素。以下display的属性使一个元素成为block-level，有block、list-item、table

block-level boxes：participate in a block formatting context的boxes。每一个block-level元素生成一个principal block-level box（主要的块级box），这个box包含了子box和内容，并且它是任意positioning scheme。有些block-level元素可能生成额外的box，除了principal box，例如list-item元素，这些额外的box相对于principal box放置。

> 除了table boxes和replaced elements之外，block-level box也是一个block container box。block container box要么只包含block-level boxes，要么创建一个inline formatting context用来只包含inline-level boxes。并非所有的block container boxes都是block-level boxes，例如non-replaced inline blocks和non-replaced table cells。是block-level并且是block containers的boxes叫做block boxes。

Anonymous block boxes：如果一个block container box里面有一个block-level的box，那么会强制里面的box变为block-level。例如下面的hello就会被强制转成block-level，称为匿名block box

```
<div>
	hello
    <p>world</p>
</div>
```

当一个inline box包含了block-level box，这个inline box和它在同一个line box的父inline box会被打破成围绕这个block-level box（和所有连续的或被collapsible whitespace或out-of-flow elements分割的block-level兄弟元素），把inline box分割成两个box（即使有可能一部分为空）。在break之前和之后的line boxes被匿名block box包围，这个block-level变成了那些匿名block boxes的兄弟元素。当这个inline box被relative positioning影响时，同时也会影响里面的block-level box。如下代码所示

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<HEAD>
<TITLE>Anonymous text interrupted by a block</TITLE>
<style>
p { display: inline; border: 1px solid #000; background: #ddd}
span { display: block }
</style>
</HEAD>
<BODY>
<P>
This is anonymous text before the SPAN.
<SPAN>This is the content of SPAN.</SPAN>
This is anonymous text after the SPAN.
</P>
</BODY>
```

##### Inline-level elements和inline boxes
inline-level elements：those elements of the source document that do not form new blocks of content; the content is distributed in lines (e.g., emphasized pieces of text within a paragraph, inline images, etc.).（源码文档中不会生成新的内容的blocks的元素；内容沿lines分布，例如段落中的强调片段、inline images等）以下的display属性能使元素成为inline-level，有inline、inline-table、inline-block。inline-level元素生成inline-level box，inline-level box参加inline formatting context。

inline-level box：is both inline-level and whose contents participate in its containing inline formatting context。一个display属性为inline的non-replaced元素生成一个inline box。不是inline boxes的inline-level boxes被称为atomic inline-level boxes，因为它们作为一个单独的不透明的box参加inline formatting context。 

Anonymous inline boxes：任何文本直接被包含在一个block container元素中（并非inline元素中），都会被当做匿名inline元素。匿名的inline box从它的父block box继承属性

##### display属性
- 值有inline、block、list-item、inline-block、table、inline-table、table-row-group、table-header-group、table-footer-group、table-row、table-column-group、table-column、talbe-cell、table-caption、none、inherit
- 初始值inline


### Postioning schemes
> 在CSS 2.1中，一个box按照三中positioning schemes进行布局，如下

- Normal flow：在CSS 2.1中，normal flow包括block-level boxes的block formatting、inline-level boxes的inline-formatting、block-level和inline-level boxes的reative positioning
- Floats：在float model中，一个box首先按照normal flow进行布局，然后从flow中取出并挪动到最左端或最右端
- Absolute positioning：在absolute positioning model中，一个box从normal flow中被整个移除（但不会对后面的兄弟元素有影响）并相遇于containing block分配一个位置

> 如果一个元素是floated、absolutely positioned或者是根元素，那么这个元素被称为out of flow，否则就是in-flow。一个元素的flow是一个由该元素和所有in-flow元素（最近的out-of-flow父元素是该元素）的集合

position属性
- 值：static、relative、absolute、fixed、inherit
	- static：box是一个normal box，按照normal flow布局。top、right、bottom、left属性不生效
    - relative：box的位置按照normal flow进行计算，然后相对于它的正常的位置进行偏移。但对于table-row-group、table-cell等元素不生效
    - absolute：box的位置由top、right、bottom、right指定，这些属性指定了相对于box的containing block的偏移量。absolutely positioned boxes被从normal flow中取出。这意味着它们不会对后面的兄弟元素的布局产生影响，同时尽管它们有margins，但是他们不和其它的margins进行collapse
    fixed：box的位置按照absolute model进行计算，除此之外，box按照一些规则固定。因为是absolute model，所以box得margins不会collapse。在手持设备、屏幕、tty、projection、tv等媒体类型中，box相对于viewport固定，并且在滚动条滚动时时不会移动；在print媒体类型中，box在每一页都会渲染，并且相对于page box固定。对于其他媒体类型，没有定义。
- 初始值：static
- 非继承属性

### Normal flow
> 在normal flow中的boxes属于一个formatting context（可能是block或inline，但不可能同时是两者）。block-level boxes参加block formatting context，inline-level boxes参加inline formatting context。

##### Block formatting contexts
Floats、absolutely positioned元素、不是block boxes的block containers（例如inline-blocks、table-cells和table-captions）和overflow属性不是visible（except when that value has been propagated to the viewport）的block boxes会为它们的contents创建新的block formatting contexts

在一个block formatting context中，boxes从containing block的顶部开始，在垂直方向一个接一个的布局。两个兄弟boxes之间的垂直距离由margin决定。在同一个block formatting context中相邻的block-level boxes的垂直margin会collapse。

在一个block formatting context中，每个box的左外边界挨着containing block的左边界（对于right-to-left formatting，正好相反）。即时在floats中也是一样，除非这个box新建了一个block formatting context。

##### Inline formatting contexts
在一个inline formatting context中，boxes从containing block的顶部开始，按水平方向一个接一个的布局。这些boxes在垂直方向可能有不同的对齐方式：按顶部或底部对齐，或者按文字的baselines对齐。包含这些组成一条线的boxes的矩形区域被称为line box。

line box的宽度由containing block和floats的表现决定，高度由line-height和vertical-align决定。

一个line box对于它所包含的boxes来说总是足够高。当一个在line box中的box比line box低时，它的垂直对齐方式由vertical-align属性决定。当几个inline-level boxes在一个line box的水平方向排不下时，它们会被分发到两个或多个vertical-stacked line boxes（垂直方向上产生多个line box来容纳这些inline-level boxes，就像栈一样）。因此，一个段落就是一个vertical-stack of line boxes。line boxes不会重叠

通常情况下，line box的左边缘挨着containing block的左边缘，右边缘挨着containing block的右边缘。但是floating boxes可能会在containing block和line box之间，因此，line box的宽度可能会由于floats而缩小。在同一个inline formatting context中的line boxes的高度通常不一致，比如有些只包含文本，有些包含一个很高的图片。

当一条线上的inline-level boxes的总宽度小于line box的宽度时，它们在水平方向的分布由text-align属性决定。如果这个属性的值是justify，用户代理会在inline boxes中拉伸字和空间（在inline-table和inline-block boxes中不会）。

当一个inline box超过了line box的宽度，它会被分割成几个boxes并且这些boxes沿着几个line boxes排布。如果一个inline box不能被分割（比如包含了一个单独的字符，或不允许break的单词，或者这个inline box被一个nowrap或pre的white-space影响），那么inline box会从line box中溢出。

当一个inline box被分割了，margins、borders和padding在分割的地方不会受影响。

inline boxes可能会因为bidirectional text processing（有些脚本中字符会从右往左写）虽然被分割成几个boxes，但仍在同一个line box中。

line box被创建是为了保持inline-level content在一个inline formatting context中。不包含文本、preserved white space、非零margins/paddings/borders的inline元素、其它的in-flow content（例如图片、inline blocks或inline tables），并且不是以preserved newline结尾的line boxes必须作为零高line box对待，目的是决定line boxes中的任何元素的位置。对于其他的目的来说，这些line boxes不存在。

##### Relative positioning
一旦box按照normal flow或floated进行布局，它就可能相对于这个位置挪，这叫做relative positioning。但是跟在挪动的box后面的box不会进行位置挪动，这就意味着relative positioning可能会引起boxes之间的重叠。如果引起了overflow属性是auto或scroll的box中的内容溢出了，用户代理必须允许用户访问这部分内容，而且由于滚动条的创建，可能会影响布局。

一个relatively positioned box保持它的normal flow size不变，包括line breaks和原来的空间。

### Floats
float就是一个box被挪动到当前line的左边或右边，直到挨上containing block或另一个float的外边缘。

如果没有足够的水平空间放float，它就会向下挪动直到能放下它或者没有其他的float。

因为float不在flow中，所以non-positioned block boxes在float box flow的垂直方向的前后被创建，就好像这个float不存在一样。但是，挨着float被创建的当前和随后的line boxes按需要被截短以保证float的margin box有足够的空间（没看懂）
