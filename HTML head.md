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

// 定义网页搜索引擎方式，通常的取值有none，noindex，nofollow，all，index和follow
<meta name="robots" content="index,follow">
```