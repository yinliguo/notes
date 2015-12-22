# 前端开发规范

## 一般规范
- 文件名以“-”分割。例：big-black-background.jpg
- 文件名以字母开头
- 资源名称全部小写
- 对文件加前缀或后缀表示特定含义。例如jquery.min.js
- 当引入资源时，不要指定协议，除非http和https都不可用。例如url(//static.example.com/images/bg.jpg)
- 文本一次缩进两个空格
- 加上合适的注释，使用注释工具JSDoc、YUIDoc
- 代码检查使用JSLint或JSHint

## HTML规范
- 避免使用XHTML以及它的属性，比如application/xhtml
- id命名以下划线分割，全部小写，class以“-”分割
- 无内容的标签不要闭合，比如<br>
- 脚本加载放到body底部，例如<script src="main.js" async></script>
- 语义化
- 图片、视频加上alt描述
- 分离html、js、css
- 在文档和模板中尽量少地引入css和js
- 不超过两张样式表
- 不超过两个脚本
- 不再元素上使用style属性
- 不使用行内脚本
- 不使用表象元素（表现css的元素，如b标签）
- 不使用表象class名
- HTML是为了展示内容，而不是解决视觉设计问题
- 省略js和css引入时的type属性
- 使用微格式进行SEO
- HTML中的引号使用双引号

## js规范：
- 将代码包裹成一个立即函数中以避免污染全局命名空间
- 使用use strict

## css规范：
- id和class应该是表示用途或目的的名称
- id一般不应该用于样式，而用于确定位置
- css选择器中避免标签名
- 使用选择器链时尽量精确，使用子选择器“>”
- 尽量使用缩写的属性
- 省略0值后面的单位
- 使用“-”作为id和class的分隔符
- 先写结构性样式，再写表现性样式
