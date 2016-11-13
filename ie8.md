# IE8 兼容问题

- array没有indexOf方法
```
if (!Array.prototype.indexOf)
{
    Array.prototype.indexOf = function(elt /*, from*/)
    {
        var len = this.length >>> 0;
        var from = Number(arguments[1]) || 0;
        from = (from < 0)
            ? Math.ceil(from)
            : Math.floor(from);
        if (from < 0)
            from += len;

        for (; from < len; from++)
        {
            if (from in this &&
                this[from] === elt)
                return from;
        }
        return -1;
    };
}
```
- string没有trim方法
```
if(typeof String.prototype.trim !== 'function') {
  String.prototype.trim = function() {
    return this.replace(/^\s+|\s+$/g, '');
  }
}
```
- form中必须使用手动点击选择文件的按钮才能提交表单，js点击file按钮选择文件无效则无法提交
- Date没有now方法
```
var now = (new Date()).getTime();
```
- js控制windows media player的ActiveX时，Player.controls中的属性和方法是无法打印出来的，但可以调用
- 事件绑定需要使用attachEvent
- 获取选中的文字时，ie8只能用document.selection.createRange()，其他则用document.getSelection()
```
if (document.getSelection) {
	return document.getSelection().toString();
} else if (document.selection) {
	reurn document.selection.createRange().text;
}
```
- IE对GET请求有缓存，需要在web服务器上设置过期时间
