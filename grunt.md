# grunt

Grunt是一款构建工具，依靠众多的插件可以实现自动化构建项目，比如合并文件、压缩文件、检查js代码……

## 安装和使用
1、 安装grunt-cli  
Grunt是基于node的软件，所以首先安装node，然后安装grunt-cli，grunt-cli是grunt的命令行工具，作用是调用与Gruntfile同一目录下的Grunt，这样就能在一个项目中安装多个Grunt版本。  
注意：在windows中全局安装完grunt-cli后可能需要手动将grunt-cli的执行文件的路径加入到path变量中  
```
npm install -g grunt-cli
```
2、 生成package.json文件  
进入项目中，使用如下命令生成package.json文件，这个文件是描述项目的文件  
```
npm init
```
3、 安装Grunt  
```
npm install grunt --save-dev
```
4、 安装所需要的插件  
grunt的插件和用法可以在[插件列表](http://www.gruntjs.net/plugins)里获得，常用的插件有grunt-contrib-concat、grunt-contrib-uglify、grunt-contrib-jshint、grunt-contrib-copy……  
```
npm install grunt-contrib-concat --save-dev
```
5、执行构建命令  
使用在gruntfile文件中定义的grunt命令来执行自动化构建  
```
grunt default
```