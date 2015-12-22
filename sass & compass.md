# sass & compass

由于CSS文件的枯燥和可维护性差，于是诞生了sass，这款工具的目的是将CSS文件以另一种可维护的形式（.scss文件）表达出来，使用的时候将其编译成css。为了更方便地编写.scss文件，于是就出现了compass这个工具库，这个库里封装了很多scss语法的工具函数。

## sass语法
- 变量  
```
$nav-color: #abcdef;
nav {      
	$width: 100px;  
	width: $width;  
	color: $nav-color; 
}
```
- 表达式  
```
$grid-cells: 20;
$cell-width: 25px;
#main {
	$main-width: $grid-cells * $cell-width;
	$main-padding: 10px;
	width: $main-width;
	padding: $main-padding;
	.sidebar {
   		width: ($main-width - $main-padding*2)/4
   }
}
```
- 嵌套规则  
```
#content {
	article {
		h1 { color: #333 }
		p { margin-bottom: 1.4em }
	}
	aside { background-color: #eee }
}
```
- 导入sass文件  
```
@import "compass/css3"
```
- 混合器  
```
@mixin rounded-corners {
	-moz-border-radius: 5px;
	-webkit-border-radius: 5px;
	border-radius: 5px;
}
.notice {
	background-color: green;
	border: 2px solid #00aa00;
	@include rounded-corners;
}
```
编译生成  
```
.notice {
	background-color: green;
	border: 2px solid #00aa00;
	-moz-border-radius: 5px;
	-webkit-border-radius: 5px;
	border-radius: 5px;
}
```
- 继承  
```
.error{
	border: 1px red;
	background-color: #fdd;
}
.seriousError {
	@extend .error;
	border-width: 3px;
}
```
编译生成
```
.error,.seriousError {
	border: 1px red;
	background-color: #fdd;
}
.seriousError {
	border-width: 3px;
}
```
- 函数  
```
abs($number)
```
```
@function grid-width($cells) {
	@return ($cell-width + $cell-padding) * $cells;
}
```
- 控制命令  
```
@for $i from 1 through 5 {
	.rating-#{$i} {
		background-image: url(/images/rating-#{$i}.png);
	}
}
```

## compass
compass提供了5种工具函数，比如重置样式、浏览器兼容、精灵图等。  
- css3
- layout
- reset
- typography
- utilities

## 安装sass和compass
sass和compass是基于ruby开发的，需要先安装ruby，国内的童鞋最好换成淘宝的源  
```
gem source -r http://rubygems.org/
gem source -a http://ruby.taobao.org/
```
接下来安装sass和compass  
```
gem install sass
gem install compass
```
