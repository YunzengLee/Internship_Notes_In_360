## 安装php5.6版本

环境（CentOS7）中已经存在php7.2，而且是之前使用yum安装的。

有关信息如下：

```bash
[root@xiejunyang core]# php -v
PHP 7.2.31 (cli) (built: May 31 2020 16:18:31) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.31, Copyright (c) 1999-2018, by Zend Technologies
[root@xiejunyang core]# whereis php
php: /usr/bin/php /usr/lib64/php /etc/php.d /etc/php.ini /usr/include/php /usr/share/php /usr/share/man/man1/php.1.gz

```

**在这个基础上安装php5.6并切换**，就需要通过编译安装（使用tgz压缩包）来安装php5.6到某个指定目录下。

以下内容参考自：[linux下编译安装php5.6](https://blog.csdn.net/t_1007/article/details/78660822)



**开始**

1. \# cd ~

2. \# wget http://mirrors.sohu.com/php/php-5.6.2.tar.gz

3. \# tar xf php-5.6.2

4. \# yum install gcc gcc-c++ libxml2 libxml2-devel libjpeg-devel libpng-devel freetype-devel openssl-devel libcurl-devel libmcrypt-devel

5. \# cd php-5.6.2

6. 配置：--prefix是安装目录，--with-config-file-path是配置文件存放目录，--with-mysql是mysql的安装目录，由于我是用yum安装的，所以不需要写-with-mysql=####。

```bash
./configure --prefix=/usr/local/php56 \
 --with-config-file-path=/usr/local/php56/etc \
 --enable-fpm --enable-pcntl --enable-mysqlnd --enable-opcache --enable-sockets --enable-sysvmsg --enable-sysvsem \
--enable-sysvshm --enable-shmop --enable-zip --enable-ftp --enable-soap --enable-xml --enable-mbstring \
--disable-rpath --disable-debug --disable-fileinfo --with-pcre-regex --with-iconv --with-zlib --with-mcrypt --with-gd --with-openssl \
--with-mhash --with-xmlrpc --with-curl --with-imap-ssl --with-mysql --with-mysqli --with-pdo-mysql
```

7. \# make

   \# make install

8. \# cp php.ini-production /usr/local/php56/etc/php.ini
   \# cd /usr/local/php56/etc （这个路径就是config-file-path）
   \# cp php-fpm.conf.default php-fpm.conf

目前已经安装成功，但是环境还没有切换。



参考[安装多个PHP版本](https://blog.csdn.net/firwind/article/details/80236928)继续进行，最终成功。过程如下：

1. 回到解压目录 php-5.6.2
   
2. 

   ```bash
   cp php.ini-development /usr/local/php56/lib/php.ini
   ```

   （这一步好像没有必要）

3. 配置php-fpm

   ```bash
   cp /usr/local/php56/etc/php-fpm.conf.default /usr/local/php56/etc/php-fpm.conf
   
   vim /usr/local/php56/etc/php-fpm.conf
   
   将listen = 127.0.0.1:9000
   改为listen = 127.0.0.1:9001
   因为php7已经占用了9000端口，所以用9001端口。
   ```

4. 配置php-fpm服务

   因为php7的服务文件为php-fpm.service，所以这里用php5-fpm.service

   ```bash
   cp sapi/fpm/php-fpm.service /usr/lib/systemd/system/php5-fpm.service
   (此时位于php5.6.21的tar包的解压文件夹下，有sapi这个目录)
   vim /usr/lib/systemd/system/php5-fpm.service
   ```

   将：

   > PIDFile=${prefix}/var/run/php-fpm.pid
   > ExecStart=${exec_prefix}/sbin/php-fpm --nodaemonize --fpm-config ${prefix}/etc/php-fpm.conf

   改成

   > PIDFile=/usr/local/php56/var/run/php-fpm.pid
   > ExecStart=/usr/local/php56/sbin/php-fpm --nodaemonize --fpm-config /usr/local/php56/etc/php-fpm.conf

   这里做的就是用刚才 PHP 5 安装路径替代 prefix 变量

5. 开启php-fpm

   重新载入 systemd

   > [root@lnmp php-5.6.21]# systemctl daemon-reload

   可以设置开机启动：

   > [root@lnmp php-5.6.21]# systemctl enable php5-fpm

   立即启动 php-fpm

   > [root@lnmp php-5.6.21]# systemctl start php5-fpm

   查看状态：

   > [root@lnmp php-5.6.21]# systemctl status php5-fpm
   > php5-fpm.service - The PHP FastCGI Process Manager
   > Loaded: loaded (/usr/lib/systemd/system/php5-fpm.service; disabled)
   > Active: active (running) since Wed 2016-05-18 18:06:40 CST; 28s ago
   > Main PID: 5867 (php-fpm)
   > CGroup: /system.slice/php5-fpm.service
   >    ├─5867 php-fpm: master process (/usr/local/php5/etc/php-fpm.conf)
   >    ├─5868 php-fpm: pool www
   >    └─5869 php-fpm: pool www
   > May 18 18:06:40 lnmp.cn systemd[1]: Started The PHP FastCGI Process Manager.
   > [root@lnmp php-5.6.21]#

6. 配置不同的nginx站点使用不同的php版本

   打开nginx配置文件，默认是nginx安装目录（/usr/local/nginx）下的conf/nginx.conf。修改server块，只需要将fastcgi_pass 127.0.0.1：9000改为127.0.0.1：9001即可。

   ![image-20200722104504835](https://github.com/YunzengLee/my_pictures/blob/master/image-20200722104504835.png?raw=true)

