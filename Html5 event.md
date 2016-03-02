## Html5 event

### window事件
- onafterprint：文档打印之后
- onbeforeprint：文档打印之前
- onbeforeunload：文档卸载之前
- onerror：发生错误
- onhaschange：文档改变时
- onload：页面加载后
- onmessage：消息被触发时
- onoffiline：离线时
- ononline：上线时
- onpagehide：窗口隐藏时
- onpageshow：窗口可见时
- onpopstate：窗口历史记录改变时
- onredo：文档执行撤销时
- onresize：窗口尺寸改变时
- onstorage：当web storage更新时
- onundo：文档执行undo时
- onunload：页面下载完成或关闭浏览器时

### form事件
> 应用于所有元素，但常见于form中

- onblur：元素失去焦点时
- onchange：元素的值改变时
- oncontextmenu：当上下文菜单被触发时
- onfocus：元素获取焦点时
- onformchange：表单改变时
- onforminput：当表单输入时
- oninput：当元素输入时
- oninvalid：元素无效时
- onreset：表单被重置时（html5不支持）
- onselec：文本被选中时
- onsubmit：表单提交时

### keyboard事件
- onkeydown：按下键时
- onkeyup：放开键时
- onkeypress：按键时

### mouse事件
- onclick：元素点击时
- ondbclick：双击时
- ondrag：元素被拖动时
- ondragend：拖动结束后
- ondragenter：被拖动到拖放目标时
- ondragleave：当元素拖离拖放目标时
- ondragover：当元素在拖放目标上被拖动时
- ondragstart：当开始被拖动时
- ondrop：当把元素放到拖放目标时
- onmousedown：当在元素上点击鼠标时
- onmousemove：当鼠标在元素上移动时
- onmouseout：当鼠标移出元素时
- onmouseover：当鼠标移动到元素上时触发
- onmouseup：当在元素上释放鼠标时
- onmousewheel：当在元素上使用滚轮时
- onscroll：当元素滚动条被滚动时

### media事件
> 适用于所有元素，但常见于媒介元素中，如audio、video、img、object、embed

- onabort：被中断时
- oncanplay：当可以播放时
- oncanplaythrough：在不用停下来缓冲的情况下触发
- ondurationchange：当媒介长度改变时
- onemptied：当发生故障，比如意外断开时
- onended：当播放完后
- onerror：文件加载期间发生错误
- onloadeddata：当媒介数据加载完成时
- onloadedmetadata：当媒介元数据加载完成时
- onloadstart：当开始加载数据时
- onpause：当暂停时
- onplay：可以播放时
- onplaying：当开始播放时
- onprogress：当正在获取媒介时
- onratechange：当播放速度改变时
- onreadystatechange：媒介数据就绪时
- onseeked：视频被移动到新的播放位置后
- onseeking：当视频将重新移动到新的位置时
- onstalled：当获取媒体数据不可用时
- onsuspend：读取媒体数据中止时
- ontimeupdate：播放位置发生变化时
- onvolumechange：当音量发生变化时
- onwaiting：需要缓冲时触发
