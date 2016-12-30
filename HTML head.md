## HTML head

### DOCTYPE
> 告诉浏览器以哪种html或者xhtml规范进行解析

- HTML 4.01 strict
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```
- HTML 4.01 Transitional
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```
- HTML 4.01 Frameset
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```
- HTML 5
```
<!DOCTYPE html>
```

### charset
> 声明文档使用的字符编码

推荐使用

```
<meta charset="utf-8">
```
之前的
```
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
```

### lang
> 

简体中文
```
<html lang="zh-cmn-Hans">
```
繁体中文
```
<html lang="zh-cmn-Hant">
```
英文
```
<html lang="en">
```

### 优先使用IE最新版和chrome
```
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
```

### 360 使用Google Chrome Frame
> 360 浏览器就会在读取到这个标签后，立即切换对应的极速核

```
<meta name="renderer" content="webkit">
<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">
```

### 禁止百度转码
> 通过百度手机打开网页时，百度可能会对你的网页进行转码，脱下你的衣服，往你的身上贴狗皮膏药的广告，为此可在 head 内添加

```
<meta http-equiv="Cache-Control" content="no-siteapp" />
```

### SEO优化
```
// 必须有title
<title>your title</title>

// 页面关键词
<meta name="keywords" content="your keywords">

// 页面描述内容
<meta name="description" content="your description">

// 定义网页作者
<meta name="author" content="author,email address">

// 定义网页搜索引擎方式，通常的取值有
none
index 正常索引
noindex 不索引
follow 抓取该页面的链接
nofollow 不抓取该页面的链接
all
<meta name="robots" content="index,follow">
```

### viewport
> viewport 可以让布局在移动浏览器上显示的更好。content的可选参数如下

- width：viewport 宽度(数值/device-width)
- height：viewport 高度(数值/device-height)
- initial-scale：初始缩放比例
- maximum-scale：最大缩放比例
- minimum-scale：最小缩放比例
- user-scalable 是否允许用户缩放(yes/no)

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### iOS添加到主屏的标题（iOS6 later）
```
<meta name="apple-mobile-web-app-title" content="标题">
```

### iOS是否启用WebApp全屏模式
> 需要添加到主屏幕后再打开即可看到全屏

```
<meta name="apple-mobile-web-app-capable" content="yes" />
```

### iOS设置状态栏的背景颜色
> 需要启用WebApp全屏模式后才能生效。content取值如下

- default：默认值
- black：黑色
- black-translucent：黑色半透明。顶部会被状态栏遮挡

```
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
```

### iOS添加到主屏的图标
```
<link rel="apple-touch-icon" href="/apple-touch-icon-57x57.png" />
// 没有效果
<link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-57x57-precomposed.png" />
```

### iOS webapp startup image
> iOS10下没有试验成功

```
<link rel="apple-touch-startup-image" sizes="640x960" href="/splash-screen-640x960.png" />
```

### favicon icon
```
<link rel="shortcut icon" type="image/x-icon" href="/favicon.ico" />
```