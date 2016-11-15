## php installation

> 此次安装使用的是centos7，php当前最新版本为7.0.13。供其他的版本的linux或php参考

- 1. 去[php官网](https://secure.php.net/downloads.php)下载最新版本的php
```
wget http://cn2.php.net/distributions/php-7.0.13.tar.bz2
```

- 2. 解压
```
tar -jxvf php-7.0.13.tar.bz2
```

- 3. 编译
```
cd php-7.0.13
./configure --enable-fpm
make
make test
make install
```
注：在configure时可能出一些错误，解决方式见FAQ

- 4. 检验，如果输入php的版本号则已正确安装
```
php-fpm -v
```

- 5. 将配置文件复制到安装位置
```
cp php.ini-development /usr/local/php/php.ini
cp /usr/local/etc/php-fpm.conf.default /usr/local/etc/php-fpm.conf
cp sapi/fpm/php-fpm /usr/local/bin
```

- 6. 其他设置参考[官方手册](https://secure.php.net/manual/zh/install.unix.nginx.php)


### FAQ
- configure: error: xml2-config not found. Please check your libxml2 installation.
> 需要安装libxml2和libxml2-devel
```
yum install -y libxml2 libxml2-devel
```