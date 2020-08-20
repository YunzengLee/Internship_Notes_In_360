## centos8上第一次安装

**CentOS8系统上的安装最终失败了。**

centos开启ssh服务

https://www.cnblogs.com/DiDiao-Liang/articles/8283686.html

centos安装LNMPhttps://blog.csdn.net/Asce_zz/article/details/84928726?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase

Nginx已经安装通过，mysql安装已经通过。

mysql在安装时需要注意，执行这个之前:

```bash
[root@localhost src]# yum install mysql-community-server
```

需要先执行这个（但是，在centos7上不用执行这个）：

```bash
yum module disable mysql
```

否则会出现错误信息：

```bash
未找到匹配的参数：mysql-community-server

错误：没有任何匹配
```





php安装：

安装依赖：libiconv  下载：ftp.gnu.org/pub/gnu/libiconv/

```bash
tar zxf libiconv-1.14.tar.gz
cd libiconv-1.14
./configure --prefix=/usr/local/libiconv
make
make install
```

make 命令如果报错，解决办法看这里：https://blog.csdn.net/weixin_34248705/article/details/92743264



## 在centos7上重新开始



按照[这里](https://blog.csdn.net/weixin_40916188/article/details/81389687?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)从头开始执行，一直到第四步的7，发现/etc目录下没有php-fpm.conf.default文件（该目录是php通过rpm源安装的默认目录，php.ini就在这个文件夹下），此时参照[这里](https://blog.csdn.net/qq_41451303/article/details/104180607?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-5.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-5.nonecase)，从四、2 开始，即配置php-fpm使nginx能解析php。

可能出现的错误【1】：配置完后重启php-fpm，这时可能无法重启，显示ERROR: [pool www] cannot get uid for user 'nginx'，此时用这个解决：[启动php-fpm报错的解决](https://www.emam.cn/index/details/id/129/cid/11.html). 然后继续配置即可。

安装mysql参考[博客2](https://blog.csdn.net/Asce_zz/article/details/84928726?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)面的安装mysql部分即可。

查看初始密码后登陆。（mysql初始密码：dsZlIHhn-0gS）

安装完成后，需要重新设置密码才能使用，[博客2](https://blog.csdn.net/Asce_zz/article/details/84928726?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)中设置的新密码不符合要求，按照mysql的默认密码要求，新设的密码必须要满足8位以上，大小写字母，数字，特殊字符混合，这几个要求。使用新的密码登陆后，可以查看mysql的密码策略并修改策略，使简单密码可用，参考[博客3](https://blog.csdn.net/hello_world_qwp/article/details/79551789)，执行

```sql
set global validate_password.policy=LOW;
```

即可，之后便能够将密码设置为‘123’这种简单的形式。



将CI4框架整合进nginx，参考[博客4](https://blog.csdn.net/j326214730/article/details/105533431)：只需在nginx.conf的server｛｝中补充

```bash
location /api {
    alias /var/www/api/public;
    try_files $uri $uri/ @api;
    location ~ \.php$ {
        include fastcgi.conf;
        fastcgi_param SCRIPT_FILENAME    $request_filename;
        fastcgi_pass        unix:/run/php/php7.2-fpm.sock;
    }
}
```

即可。

参考上文，我的具体设置为：

```bash
server {
        listen       80;
        server_name  localhost;
        #charset koi8-r;
        #access_log  logs/host.access.log  main;
	# these rows are set into # by myself, to avoid to conflict with the next location rows
        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

location /test {
   alias html/test/MyCIDemo/public;
   try_files $uri $uri/ @api;
    location ~ \.php$ {
     include fastcgi.conf;
     fastcgi_param SCRIPT_FILENAME $request_filename;
     fastcgi_pass  127.0.0.1:9000;
   }
} 
# 我将CI4项目目录MyCIDemo放在了/usr/local/nginx/html/test目录下，此处的alias需要设置为public目录（里面放着入口php，即CI自带的index.php）

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #这一段是我从别的博客上拿来的，与CI框架项目无关，可以直接访问单独的某个php文件
        location ~ \.php$ {
            root           html/test;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
}
```

这样设置之后，在浏览器输入localhost/test/index.php/controller/method即可运行CI框架，

也可以输入localhost/test/xxx.php直接访问（位于html/test/MyCIDemo/public下的）其他PHP文件。

## 遇到的BUG

### 1 selinux

在linux环境中，CI框架在nginx上运行时，先进行$app->run()初始化，这时项目目录下的writable/cache文件夹由于不可写，会抛出一个异常。【CodeIgniter\Cache\Exceptions\CacheException::forUnableToWrite】。相关代码如下，$file就是cache的文件夹路径

![image-20200706164937686](https://github.com/YunzengLee/my_pictures/blob/master/md_pic/image-20200706164937686.png?raw=true)

看到代码上的注释，猜想需要关闭php的安全模式（在php.ini中令safe_mode=Off），。但是在php5.4之后 就移除了safe_mode，php.ini中就没有safe_mode这一项了，但CI框架还是会读取，我尝试在php.ini中添加上safe_mode=Off。但是不管用。**事实证明** DIRECTORY_SEPARATOR===‘/’ 是TRUE的，因此与后面的ini_get（‘safe_mode’）无关。出现异常是因为 is_writable函数返回了false。

函数is_writable找不到函数体（可能是底层函数），其说明为：Returns TRUE if the filename exists and is writable. The filename argument may be a directory name allowing you to check if a directory is writable. 
Keep in mind that PHP may be accessing the file as the user id that the web server runs as (often 'nobody'). Safe mode limitations are not taken into account. （如果文件名存在且可写，则返回TRUE。filename参数可以是允许您检查目录是否可写的目录名。请记住，PHP可能以web服务器运行时的用户id（通常为“nobody”）访问该文件。不考虑安全模式限制。）

猜想可能是在上文提到的错误【1】时设置了php_fpm的用户为nginx，用户组为nginx，这个用户可能没有高的权限导致无法写文件。接下来尝试修改nginx的权限。修改了对应目录（也就是is_writable函数的参数$file,运行时是项目目录下的writable/cache文件夹）的权限为777（所有用户可读可写可执行，最高权限）（或者把这个文件夹的所属用户和用户组改为nginx），但是还是不管用。

原因已找到：linux系统中selinux默认开启的原因，在线上环境中selinux是关闭的。[如何设置selinux](https://blog.csdn.net/zhoushengtao12/article/details/95346903)

修改selinux之后，还需要修改CI框架对cache的目录设置，将这个目录设置为自定义的一个目录（设置为了/data/ci_cache。设为/usr之下或者/tmp之下好像是不行的，即使已经将cache文件夹的权限设为777也不行。这两个目录最好不要设为777的权限，因此自定义一个文件夹来保存cache）

方法：修改app/Config/Cache.php：

```php
public $storePath =  '/data/ci_cache/';
```

修改/data/ci_cache文件夹的属主和属组为上文提到的错误【1】中设置的php_fpm的用户和用户组。（线上环境中，这个用户和用户组一般是nobody，一个无需自己创建的用户和用户组，没有太高的权限，只给他访问某些文件夹的权限，这样可以保证项目运行的安全性）

### 2 数据库密码

连接数据库错误【 The server requested authentication method unknown to the client】 的[解决办法](https://blog.csdn.net/it_users/article/details/84933225)。（本质上是取消数据库密码的加密，设置为一个简单密码）

过程中可能会因为不满足密码要求而无法修改密码，此时就需要参考[博客3](https://blog.csdn.net/hello_world_qwp/article/details/79551789)更改密码要求。（将password_validate.policy设为LOW即可）



### 3 大小写问题

在linux中，类似于

```php
echo view('templates/header', $data);
echo view('news/index', $data);
echo view('templates/footer');
```

这种php  CI框架中的代码，view函数的第一个参数是文件夹路径（templates/header代表Templates/Header.php这个文件，剩下两行类似），这个在win中是不区分大小写的，也就是说，文件夹实际名字和参数相比可以有大小写的不一致，但是linux中必须一致，否则报错。

### 4 form_open函数

CI框架封装了一个 form_open的函数：

php===》 echo form_open('email/send')  
 等同于  
 html===》 <form method="post" action="http://example.com/index.php/email/send" />

如果这个函数中传入的字符串参数比如‘email/send’，不是一个完整的url，就会自动补充为完整的url,方式是直接把app/Config/App.php中的以下两个变量拼接。

```php
public $baseURL = 'http://localhost:8080/';

public $indexPage = 'index.php';
```

比如，如果form_open中传入‘email/send’那么最终请求的url为‘http://localhost:8080/index.php/email/send’。

## 启动方式

启动php-fpm：

1. service php-fpm start (restart 重启)
2. php的安装目录下的/sbin下执行./php-fpm

php的配置文件在/etc/php.ini和/etc/php.d 下

php-fpm的配置文件在/etc/php-fpm.conf和/etc/php-fpm.d 下

启动mysql：

1. service mysqld start
2.  systemctl start mysqld.service

配置文件在/etc/my.conf 和 /etc/my.cnf.d下

启动nginx：

1. nginx的安装目录下的/sbin下执行./nginx [-t 测试能否启动/ -s reload 重新读取配置文件并重启] 
2. 配置文件在安装目录下的conf/nginx.conf





## 编译安装php5.6版本

环境中已经存在php7.2，而且是之前使用yum安装的。

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

6. 配置：--prefix是安装目录，--with-config-file-path是配置文件存放目录，--with-mysql是mysql的安装目录，由于我是用yum装的，所以不需要写-with-mysql=####。

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

8. \#cp php.ini-production /usr/local/php56/etc/php.ini
   \#cd /usr/local/php56/etc （这个路径就是config-file-path）
   \#cp php-fpm.conf.default php-fpm.conf

目前已经安装成功，但是环境还没有切换。



参考[安装多个PHP版本](https://blog.csdn.net/firwind/article/details/80236928)继续进行，最终成功。过程如下：

1. ```bash
   cp php.ini-development /usr/local/php56/lib/php.ini
   ```

2. 配置php-fpm

   ```bash
   cp /usr/local/php5/etc/php-fpm.conf.default /usr/local/php5/etc/php-fpm.conf
   
   vim /usr/local/php5/etc/php-fpm.conf
   
   将listen = 127.0.0.1:9000
   改为listen = 127.0.0.1:9001
   因为php7已经占用了9000端口，所以用9001端口。
   ```

3. 配置php-fpm服务

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

4. 开启php-fpm

   重新载入 systemd

   > [root@lnmp php-5.6.21]# systemctl daemon-reload

   可以设置开机启动：

   > [root@lnmp php-5.6.21]# systemctl enable php5-fpm

   立即启动 php-fpm

   > [root@lnmp php-5.6.21]# systemctl start php5-fpm

   查看状态：

   > [root@lnmp php-5.6.21]# systemctl status php5-fpm
   > php5-fpm.service - The PHP FastCGI Process Manager
   >   Loaded: loaded (/usr/lib/systemd/system/php5-fpm.service; disabled)
   >   Active: active (running) since Wed 2016-05-18 18:06:40 CST; 28s ago
   >  Main PID: 5867 (php-fpm)
   >   CGroup: /system.slice/php5-fpm.service
   >       ├─5867 php-fpm: master process (/usr/local/php5/etc/php-fpm.conf)
   >       ├─5868 php-fpm: pool www
   >       └─5869 php-fpm: pool www
   > May 18 18:06:40 lnmp.cn systemd[1]: Started The PHP FastCGI Process Manager.
   > [root@lnmp php-5.6.21]#

5. 配置不同的nginx站点使用不同的php版本

   打开nginx配置文件，默认是nginx安装目录（/usr/local/nginx）下的conf/nginx.conf。修改server块，只需要将fastcgi_pass 127.0.0.1：9000改为127.0.0.1：9001即可。

   ![image-20200722104504835](https://github.com/YunzengLee/my_pictures/blob/master/md_pic/image-20200722104504835.png?raw=true)



## [安装PHP的MongoDB扩展](https://www.runoob.com/mongodb/mongodb-install-php-driver.html)

关于mongodb有两个扩展，分别名为mongo和mongodb，都需要安装，安装方式是一样的。

安装后重启php-fpm。

## 小错误

报错：网页显示502，查看nginx的error.log有错误信息为：upstream sent too big header while reading response header from upstream

解决方法：https://www.cnblogs.com/jackluo/p/3410739.html



项目已经可以成功运行，但界面一直显示warning：没有设置日期的警告。修改php.ini中的date.timezone="Asia/Shanghai" 然后重启php-fpm解决。



## Linux CentOs7安装Git

### 一：先启用EPEL存储库

系统位数不同命令也不同，以下列举了CentOS 7 64位和32位的启用EPEL存储库命令，大家按照系统版本选择执行即可（纳尼？不清楚自己是多少位系统？参考：[如何查看Linux系统位数？32位或64位？](http://www.linuxbaike.com/bit3264/)）。
 RHEL/CentOS 7 64位执行以下命令：
 执行命令：`wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm`
 执行命令：`rpm -ivh epel-release-latest-7.noarch.rpm`



RHEL/CentOS 7 32位执行以下命令：
 执行命令：`get http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm`
 执行命令：`rpm -ivh epel-release-6-8.noarch.rpm`



### 二：安装git命令



执行命令：`yum install -y git`

 然后等待系统自动安装吧，当提示“Complete!”，说明已经安装成功，可以使用git命令啦！使用`git --version`查看Git版本。



几个环境变量文件的区别：[Linux中/etc/profile、~\.bash_profile环境变量配置](https://blog.csdn.net/aafeiyang/article/details/82876310)





## [Linux定时任务命令](https://man.linuxde.net/crontab)



## win10下的网络映射驱动器

作用：把远程电脑的某个文件夹映射到本地磁盘，像操作本地磁盘一样操作远程文件。（这样就可以使用本地IDE打开服务器上的代码并修改）

方法：

1. cmd中输入

   ```bash
   start \\10.12.13.66  #输入要连接的远程电脑（服务器）的ip
   ```

2. 在弹出框中输入用户名和密码才能登陆

3. 登录后可以看到所有文件夹，右键点击某一个，然后点击【映射网络驱动器】。接下来进行相关设置就完了。

