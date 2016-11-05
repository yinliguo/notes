# nginx

## Linux下安装nginx
1. 到[nginx官网](http://nginx.org/)下载nginx源码，例如nginx-1.11.5.tar.gz
2. 解压源码`tar -zxvf nginx-1.11.5.tar.gz; cd nginx-1.11.5`
3. 编译源码并安装`./configure; make; make install`，安装后一般是在/usr/local/nginx目录中

### FAQ
> Q: ./configure时报错缺少pcre  
A: 下载pcre-8.38.tar.gz(ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/)并解压，然后在nginx目录中使用`./configure --with-pcre={解压后的pcre路径}`
