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

##### Run-in boxes