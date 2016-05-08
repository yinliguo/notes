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

- 根元素所在containing block是一个叫initial containing block的矩形。对于continuous media而言，它拥有viewport的尺寸并且是在canvas origin中是固定的。它是paged media的页面区域。这个initial containing block的direction属性和根元素一致。
- 对于其它元素而言，如果这个元素的position属性是relative或static，这个containing block由最近的block container父box的content边缘组成
- 如果这个元素的position属性是fixed，那么在continuous media或paged media的paged area中，containing block由viewport创建
- 如果元素的position为absolute，那么containing block由最近的position属性为absolute/relative/fixed的父元素创建，如果没有这样的父元素，containing block就是initial containing block
	- 如果父元素是一个inline元素，containing block就是环绕这个元素生成的第一个和最后一个inline boxes的padding boxes的bounding box。如果这个inline元素沿着多条线被分割，那么这个containing block就是undefined
    - 否则containing block由父元素的padding edge组成

在paged media中，一个absolutely positioned元素会忽略page breaks（就像这个文档是连续的），相对于它的containing block定位。这个元素可能会在随后的几个页面中被打断。

对于absolutely positioned content（在一个页面的位置，而不是正在布局的当前页面，或者解析解析成已经渲染或打印完成的当前页面的位置）而言，打印机可能按以下方式处理content：
- 放到当前页的另一个位置
- 放到随后的页面
- 忽略它



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

一个line box挨着一个float：存在一个垂直的的位置满足一下四个条件（没看懂）
- 在line box的顶部或其下方
- 在line box的底部或其上方
- 在float的top margin edge下方
- 在float的bottom margin edge上方

> 这意味着outer height为0或负值的floats不会缩短line boxes

如果一个被截短的line box太小容不下任何content，那么这个line box会向下挪动（它的宽度重新计算）直到能够容纳一些content或者不再有float。在一个floated box之前的current line中的content会在float另一边的同一条line中被reflowed。换句话说，如果inline-level boxes被放到左float之前，并且float可以在同一条line中摆放，那么这个inline-level boxes会被移动到这个float的右边，反之同理。示例代码如下
```
<div>
	<span style="display: inline-block; width: 200px;height: 20px; background: green;"></span>
	<div style="width: 500px; height: 30px; background: red; float: left;"></div>
</div>
```

table的border box、block-level replaced元素和在normal flow中创建了一个新的block formatting context的元素（例如一个overflow属性不为visible的元素）绝不会与在同一个block formatting context中的floats的margin box重叠。如果有必要，实现应该把上面提到的元素放到floats的下面，但是如果有足够的空间，可能会把它放到与floats相邻的位置。甚至使上面提及的元素的border box比定义的更窄。代码示例如下
```
p { width: 10em; border: solid aqua; }
span { float: left; width: 5em; height: 5em; border: solid blue; }

...

<p>
  <span> </span>
  Supercalifragilisticexpialidocious
</p>
```

float属性：指定了一个box应该向左还是向右浮动，或者不浮动。适用于不是absolutely positioned的元素

clear属性：标志元素的box的哪一边不能挨着之前的floating box。clear属性不考虑在该元素里面的的floats和在其他block formatting contexts中的floats。

### Absolute positioning
在absolute positioning model中，一个box很明确地相对于它的containing block偏移。它被从normal flow中整个移除（对后面的兄弟元素没有影响）。一个absolutely positioned box为normal flow子元素和absolutely（但不是fixed）positioned后代创建了一个新的containing block。

However, the contents of an absolutely positioned element do not flow around any other boxes. They may obscure the contents of another box (or be obscured themselves), depending on the stack levels of the overlapping boxes.（没看懂）

absolutely positioned元素的position属性是absolute或fixed。

##### Fixed position
fixed positioning是absolute positioning的子范畴，唯一的区别就是对于一个fixed positioned box而言，containing block由viewport创建。对于continuous media来说，当文档滚动时，fixed box不会移动

### Relationships between 'display', 'position', and 'float'
这三个属性按照以下顺序相互影响
- 如果display的值为none，那么position和float都无效，元素也不会生成box
- 如果position的值为absolute或fixed，这个box是absolutely positioned，float的值为none，display的值按照下面的表格进行设定。box的位置由top、right、bottom、left和containing block决定
- 如果float的值部位none，box会浮动并且display的值按照下面的表格进行设定
- 如果是根元素，display按照下面的表格设定（在CSS 2.1中，没有规定list-item值是否会变成一个block或list-item）
- display属性按照指定的生效

Specified value                        (Computed value)  
++++++++++++++++++++++++++++++++++++++++++++++++++++++++  
inline-table                               (table)  
++++++++++++++++++++++++++++++++++++++++++++++++++++++++  
inline,table-row-group,table-column,       (block)  
table-column-group,table-header-group,  
table-footer-group,table-row,table-cell,  
table-caption,inline-block  
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++  
others                                      (same as specified)  

### Layered presentation
z-index属性：对于一个positioned box来说，该属性指定了：
- 当前stacking context中的stack level
- 这个box是否创建了一个stacking context

The order in which the rendering tree is painted onto the canvas is described in terms of stacking contexts. Stacking contexts can contain further stacking contexts. A stacking context is atomic from the point of view of its parent stacking context; boxes in other stacking contexts may not come between any of its boxes.（渲染树绘制到画布上的顺序是在stacking context中描述的。stacking contexts能包含更深一层的stacking contexts。一个stacking context对于它的上一级stacking context来说是原子性的，不可分割的；其它的stacking contexts中的boxes不会到该stacking context中的boxes里）

每一个box都属于一个stacking context。在一个给定的stacking context中的每一个positioned box都有一个整数的stack level，标志它相对于同一个stacking context中的其他boxes在Z轴的位置。stack level越大，就越在前面。stack level可能有负值。stack level相同的话就按照文档树中的顺序从后到前摆放。

根元素组成了根stacking context。其它stacking contexts由positioned元素（包括relatively positioned元素，有z-index的computed值，但不是auto）生成。对于containing block来说，stacking contexts并不是必须的。

在每个stacking context中，按照下面的顺序从后到前的顺序进行渲染
- 1.组成stacking context元素的background和borders
- 2.负值的stack levels的子stacking contexts（从小到大）
- 3.in-flow、non-inline-level、non-positioned后代元素
- 4.non-positioned floats
- 5.in-flow、inline-level、non-positioned后代元素，包括inline tables和inline blocks
- 6.stack level为0的子stacking context和stack level为0的positioned后代元素
- 7.stack level为正数的子stacking contexts（从小到大）

> 在每一个stacking context中，stack level 0的positioned元素（6）、non-positioned floats（4）、inline blocks（5）和inline tables（5）被绘制就好像这些元素自己生成了新的stacking context，除了它们的positioned后代和子stacking contexts。绘制的顺序是递归的。

示例代码如下
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<HEAD>
<TITLE>Anonymous text interrupted by a block</TITLE>
<style>
* {
    margin: 0;
    padding: 0;
    border: 0;
}
#div1{
	width: 800px;
	background: green;
	border: 1px solid dashed;
	position: relative;
	z-index: 0;
}
#div2{
	width: 800px;
	height: 100px;
	background: red;
	position: absolute;
	z-index: -10;
	text-align: right;
}
#div3{
	width: 700px;
	height: 50px;
	background: silver;
	position: absolute;
	z-index: -5;
	text-align: right;
}
#div4{
	width: 600px;
	height: 60px;
	background: maroon;
}
#div5{
	width: 200px;
	height: 400px;
	float: left;
	background: purple;
}
#div6{
	display: inline-block;
	width: 600px;
	height: 500px;
	background: fuchsia;
}
#div7{
	width: 500px;
	height: 50px;
	position: absolute;
	z-index: 0;
	background: teal;
	top: 200px;
}
#div8{
	height: 70px;
	background: yellow;
	z-index: 0;
}
#div9{
	width: 600px;
	height: 400px;
	background: blue;
	z-index: 2;
	position: absolute;
	top: 220px;
	left: 100px;
}
</style>
</HEAD>
<BODY>
	<div id="div1">
		div1
		<div id="div2">div2</div>
		<div id="div3">div3</div>
		<div id="div4">div4</div>
		<div id="div5">div5</div>
		<div id="div6">div6</div>
		<div id="div7">div7</div>
		<div id="div8">div8</div>
		<div id="div9">div9</div>
	</div>
</BODY>
```

> Since an element with opacity less than 1 is composited from a single offscreen image, content outside of it cannot be layered in z-order between pieces of content inside of it. For the same reason, implementations must create a new stacking context for any element with opacity less than 1. If an element with opacity less than 1 is not positioned, implementations must paint the layer it creates, within its parent stacking context, at the same stacking order that would be used if it were a positioned element with ‘z-index: 0’ and ‘opacity: 1’. If an element with opacity less than 1 is positioned, the ‘z-index’ property applies as described in [CSS21], except that ‘auto’ is treated as ‘0’ since a new stacking context is always created. See section 9.9 and Appendix E of [CSS21] for more information on stacking contexts. The rules in this paragraph do not apply to SVG elements, since SVG has its own rendering model ([SVG11], Chapter 3).（由于opacity小于1的元素是由一个屏幕外的图片合成的，在它外面的内容不能被与它里面的内容按照z-order进行层叠显示。由于这个原因，实现必须为opacity小于1的元素创建一个新的stacking context。如果一个opacity小于1的元素不是定位元素，实现必须在它的父stacking context中绘制它创建的层，如果它是一个z-index属性为0且opacity为1的定位元素，会使用同样的stacking order。如果一个opacity小于1的元素是定位元素，z-index属性按照CSS 2.1描述的使用，除了auto作为0使用，因为总司会创建一个新的stacking context。）

### width属性
width指定了box的content width
- 值为length、百分比、auto、inherit，百分比是相对于containing block
- 初始值为auto
- 非继承属性

> width属性不应用于non-repaced inline元素。non-replaced inline元素的content width由在它里面的渲染的内容决定（before any relative offset of children）

### Calculating widths and margins
width、margin-left、margin-right、left、right属性的值依赖box的类型和其它的box。（用于布局的值有时候作为used value参考）。按原理来说，对于被一些更合适的值替换的auto和基于containing block计算的百分比，使用的值应该和computed value一样，但是有一些意外。下面的这些情况应该被区分
- 1.inline non-replaced元素
- 2.inline replaced元素
- 3.normal flow中的block-level non-replaced元素
- 4.normal flow中的block-level replaced元素
- 5.floating non-replaced元素
- 6.floating replaced元素
- 7.absolutely positioned non-replaced元素
- 8.absolutely positioned replaced元素
- 9.normal flow中的inline-block non-replaced元素
- 10.normal flow中的inline-block replaced元素

> 下面计算width得到的值是一个暂时的值，可能会依赖min-width和max-width而重新计算很多次

##### 1.inline non-replaced元素
width属性不生效margin-left或margin-right的值如果是auto就被计算成0

##### 2.inline replaced元素
margin-left或margin-right的值如果是auto就计算成0

如果height和width都有auto的computed value，并且这个元素有一个本身的宽度，那么这个资深的宽度就是width的used value；如果这个元素没有自身的宽度，但是有一个自身的高度和自身的比例，又或者width有auto的computed value，height有其他的computed value，这个元素有一个资深的比例，那么width的used value是：(used height) * (intrinsic ratio)

如果height和width都有auto的computed value，并且这个元素有自身的比例但没有自身的width和height，那么在CSS 2.1中，width的used value是undefined。但建议如果containing block的width不依赖replaced元素的width，那么width的used value可以从normal flow中的block-level non-replaced元素的约束方程中计算得到。

否则，如果width有一个auto的computed value，并且元素有自身的宽度，然后这个资深的宽度就是width的used value。

否则，如果width有auto的computed value，但是上面的条件都没有满足，那么width的used value变成300px。如果300px对于设备来说太宽了，用户代理应该使用有2:1比例的最大矩形的宽去适应设备。

##### 3.normal flow中的block-level non-replaced元素
下面的约束必须hold在其他的属性的used values之间  
margin-left + border-left-width + padding-left + width + padding-right + border-right-width + margin-right = containing block的width

如果width不是auto并且border-left-width + padding-left + width + padding-right + border-right-width（加上不是auto的margin-left和margin-right）比containing block的宽度大，那么按照下面的规则，margin-left或margin-right的auto值都会当做0。

如果上面所有的属性都有一个不为auto的computed value，这些值被称为over-constrained。它们的used values之中的一个将会不同于computed value。如果containing block的direction属性是ltr，那么margin-right的指定值被忽略并且值被计算so as to make the equality true。如果direction属性是rtl，那么margin-left的值也像上面那样处理。（没看懂）

如果正好有一个值被指定为auto，its used value follows from the equality.(应该是自动填充剩余空间)

如果margin-left和margin-right都是auto，那么他们的used values是相等的。这个元素相对于containing block居中。（也就是我们常用的margin: 0 auto）

##### 4.normal flow中的block-level replaced元素
width的used value跟inline replaced元素一样。margins跟non-replaced block-level元素一样。

##### 5.floating non-replaced元素
如果margin-left或margin-right是auto，那么它们的used value是0

如果width是auto，那么used value是一个可伸缩的width

可伸缩的width的计算类似于使用自动表格布局算法计算一个表格单元格的宽度。粗略地说，通过不打断lines格式化内容来计算preferred width，同时计算preferred最小宽度，例如通过尝试所有的line break。CSS 2.1不定义准确的算法。第三，找到可用的width：在这种情况下，containing block的width减去margin-left、border-left-width、padding-left、padding-right、border-right-width、margin-right的used value，和任何相关的滚动条的宽度

然后缩小并适应的宽度是：min(max(preferred minimum width, available width), preferred width)

##### 6.floating replaced元素
如果margin-left或者margin-right是auto的computed value，那么used value是0。width的used value的计算方式跟inline replaced元素一样

##### 7.absolutely positioned non-replaced元素
- static-position containing block是一个假设的box的containing block。这个box是这个元素（如果它指定position属性的值为static，float属性的值为none）的第一个box。这个假设的计算可能需要为display属性声明一个不同的computed value。
- left属性的static position是从containing block的左边缘到假设的box的left margin edge（这个假设的box是这个元素的第一个box，并且这个元素的position属性是static，float属性为none）。如果假设的box在containing block的左边，这个值就是负的。
- right属性的static position是从containing block的right edge到这个假设的box的right margin edge。如果这个假设的box在containing block的左边，这个值是正的。

But rather than actually calculating the dimensions of that hypothetical box, user agents are free to make a guess at its probable position.（没看懂）

为了计算static position，fixed positioned元素的containing block代替viewpot称为initial containing block。and all scrollable boxes should be assumed to be scrolled to their origin.（没看懂）

决定used value的约束如下：  
left + margin-left + border-left-width + padding-left + width + padding-right + border-right-width + margin-right + right = containing block的width

如果left、width、right的值都是auto，那么首先将值为auto的margin-left和margin-right的值设置为0，然后，如果创建了static-position的元素的direction属性是ltr，那么设置left to the static position，并且应用下面的第三条规则；否则设置right to the static position并应用第一条规则。

如果left、width、right都不为auto的情况下，如果margin-left和margin-right都是auto，在两个margin相等的情况下解方程，除非它们是负的，在这种情况下当containing block的direction属性是ltr，设置margin-left为0并解出margin-right。如果margin-left和margin-right中有一个是auto，解出这个值。如果值是over-constrained，忽略这个值（如果containing block的direction为rtl，忽略left，否则忽略right）并解出这个值。

否则，将margin-left和margin-right设置为0，并选择下面的一条规则。
- 1.left和width是auto，right不是auto，那么width会收缩，然后解出left
- 2.left和right是auto，width不是auto，如果元素（创建static-position containing block）的direction属性是ltr，就设置left to the static position，否则设置right to the static position。然后解出left或right
- 3.width和right是auto，left不是auto，那么width会收缩，解出right
- 4.left是auto，width和right不是auto，解出left
- 5.width是auto，left和right不是auto，解出width
- 6.right是auto，left和width不是auto，解出right

收缩width是：min(max(preferred minimum width, available width), preferred width)

##### 8.absolutely positioned replaced元素
同上面一样，但规则如下
- 1.width的used value和inline replaced元素一样。如果margin-left或margin-right指定为auto，它的used value由下面的规则决定
- 2.如果left和right都是auto的情况下，如果元素（创建了static-position containing block）的direction属性为ltr，设置left to the static position；如果direction属性为rtl，设置right to the static position。
- 3.如果left和right其中一个是auto，替换margin-left或margin-right的auto为0
- 4.如果margin-left和margin-right仍然是auto，那么在两个margin必须相等的条件下解方程，除非它们是负值，在这种情况下，当containing block的direction是ltr，设置margin-left为0，然后解出margin-right，相反同理。
- 5.如果left是auto，那么解出这个值
- 6.如果值是over-constrained，忽略这个值（如果direction是rtl就忽略left，如果是ltr就忽略right）并解出这个值

##### 9.normal flow中的inline-block non-replaced元素
如果width是auto，那么它的used value是收缩width，就像floating元素

margin-left和margin-right的auto的computed value变成0（used value）

##### 10.normal flow中inline-block replaced元素
同inline replaced元素

### min-width和max-width
这两个属性允许用户约束内容的宽度在一个特定的区域内。
- 值为lenght、百分比、inherit。百分比是相对于containing block的宽度，不能为负值
- min-width的初始值为0，max-width的初始值为none
- 非继承属性

在CSS 2.1中这两个属性对tables、inline tables、table cells、table columns、column groups是未定义的

下面的算法描述了两个属性如何影响used value：
- 1.不使用min-width和max-width来计算出width的一个临时的used value
- 2.如果这个临时的值比max-width大，那么使用max-width作为width的computed value，利用上面的规则再计算一次。
- 3.如果得到的width比min-width小，那么久使用min-width作为computed value再计算一次

### height属性
height属性指定了boxes的content height
- 值为length、百分比、auto、inherit。百分比是相对于containing block
- 初始值为auto
- 非继承属性

height对于non-replaced inline元素无效

### calculating heights and margins
计算不同类型的box的top、margin-top、height、margin-bottom和bottom是有区别的
- 1.inline non-replaced元素
- 2.inline replaced元素
- 3.normal flow中的block-level non-replaced元素
- 4.normal flow中的block-level replaced元素
- 5.floating non-replaced元素
- 6.floating replaced元素
- 7.absolutely positioned non-replaced元素
- 8.absolutely positioned replaced元素
- 9.normal flow中的inline-block non-replaced元素
- 10.normal flow中的inline-block replaced元素

##### 1.inline non-replaced元素
height属性不生效。content area的高度应该基于字体，但是这个规范中没有指定实现方式。例如，一个用户代理可能会用em-box或者字体的ascender and descender。

垂直方向的padding、border和margin开始于content area的顶部和底部，并且和line-height没有关系。但是当计算line box的高度时只能用line-height。

##### 2.normal flow中的inline replaced元素、block-level replaced元素、inline-block replaced元素和floating replaced元素
