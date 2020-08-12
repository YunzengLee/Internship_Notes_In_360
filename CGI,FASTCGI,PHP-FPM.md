

在网站的整体架构中，web服务器（如nginx、apache）只是内容的分发者，对客户端的请求做应答。

如果请求的是html这类静态页面，web服务器就去找对应的文件，返回给客户端。

如果请求的是Index.php这类动态页面，web服务器就根据配置文件知道这个不是静态文件，则调用php解释器进行处理然后返回。

![image-20200807112754697](https://github.com/YunzengLee/my_pictures/blob/master/image-20200807104417461.png?raw=true)

## CGI

### 是什么

1. Common Gateway Interface 通用网关接口。
2. 可以用php、perl、tcl等语言编写
3. 一般说的CGI是指用各种语言编写的实现该功能的程序。

### 工作原理

1. web server收到index.php这类动态请求，就启动对应的CGI程序（php的解析器）
2. php解析器会解析php.ini配置文件，初始化运行环境，然后处理请求，完成后按照CGI规定格式返回给web server
3. web server将结果返回浏览器

### 特点

1. 高并发时性能差

   CGI程序对每一次web请求都有启动退出过程，（每次HTTP服务器遇到动态请求时都需要重新启动脚本解析器来解析php.ini，重新载入全部DLL扩展并重初始化全部数据结构，然后把结果返回给HTTP服务器），很明显，这样的接口方式会导致php的性能很差，在处理高并发访问时，几乎是不可用的。

2. 传统的CGI接口方式安全性差

3. CGI对php.ini的配置敏感，在开发和调试时方便

### 应用领域

CGI为每一次请求增加一个进程，效率很低，所以基本已经不在生产部署时采用。但由于CGI对php配置的敏感性，通常被用在开发和调试阶段

## FastCGI

CGI程序性能差，安全性低，催生了FastCGI。

1. Fast CGI 快速通用网关接口
2. 致力于减少网页服务器与CGI程序之间互动的开销，从而使可以同时处理更多的网页请求（提高并发访问）。
3. 一般说的FastCGI指的也是用各种语言编写的能实现该功能的程序。

### 工作原理

1. Web Server启动同时，加载FastCGI进程管理器（nginx的php-fpm或者IIS的ISAPI或Apache的Module)
2. FastCGI进程管理器读取php.ini配置文件，对自身进行初始化，启动多个CGI解释器进程(php-cgi)，等待来自Web Server的连接。
3. 当Web Server接收到客户端请求时，FastCGI进程管理器选择并连接到一个CGI解释器。Web server会将相关环境变量和标准输入发送到FastCGI子进程php-cgi进行处理
4. FastCGI子进程完成处理后将数据按照CGI规定的格式返回给Web Server，然后关闭FastCGI子进程或者等待下一次请求。

### 进程管理方式

Fastcgi会先启一个master，解析配置文件，初始化执行环境，然后再启动多个worker。当请求过来时，master会传递给一个worker，然后立即可以接受下一个请求。这样就避免了重复的劳动，效率自然提高。而且当worker不够用时，master可以根据配置预先启动几个worker等着；当然空闲worker太多时，也会停掉一些，这样就提高了性能，也节约了资源。这就是fastcgi的对进程的管理。

### 特点

1. 支持大多数语言进行编写（C、C++、Java、PHP、Perl、Python、Ruby等），支持大多数主流web服务器（nginx，apache，lighttpd等）

2. 接口方式采用C/S结构，可以将web服务器和脚本解析服务器分开，独立于web服务器运行，提高web服务器的并发性能和安全性：

      提高性能：这种方式支持多个web分发服务器和多个脚本解析服务器的分布式架构，同时可以在脚本解析服务器上启动一个或者多个脚本解析守护进程来处理动态请求，可以让web服务器专一地处理静态请求或者将动态脚本服务器的结果返回给客户端，这在很大程度上提高了整个应用系统的性能。

      提高安全性：API方式把应用程序的代码与核心的web服务器链接在一起，这时一个错误的API的应用程序可能会损坏其他应用程序或核心服务器，恶意的API的应用程序代码甚至可以窃取另一个应用程序或核心服务器的密钥，采用这种方式可以在很大程度上避免这个问题。

3. FastCGI程序在修改php.ini配置时可以进行平滑重启加载新配置

      所有的配置加载都只在FastCGI进程启动时发生一次，每次修改php.ini配置文件，只需要重启FastCGI程序（php-fpm等）即可完成平滑加载新配置，已有的动态请求会继续处理，处理完成关闭进程，新来的请求使用新加载的配置和变量进行处理

4. FAST-CGI是较新的标准，架构上和CGI大为不同，是用一个驻留内存的服务进程向网站服务器提供脚本服务。像是一个**常驻(long-live)型的CGI**，维护的是一个进程池，它可以一直执行着，只要激活后，不会每次都要花费时间去fork一次（这是CGI最为人诟病的fork-and-execute 模式），速度和效率比CGI大为提高，是目前的主流部署方式。

5. 因为是在内存中同时运行多进程，所以会比CGI方式消耗更多的服务器内存，每个PHP-CGI进程消耗7至25兆内存，在进行优化配置php-cgi进程池的数量时要注意系统内存，防止过量。

## PHP-CGI和PHP-FPM

### PHP-CGI

很多地方说：PHP-CGI是PHP自带的FastCGI管理器，目前还没找到最原始的出处，以我的理解和经验来看这话有点毛病，我认为应该是：使用php实现CGI协议的CGI程序，可以用来管理php解释器，如果有异议可以和我探讨下。。。

使用如下命令可以启动PHP-CGI：

```
php-cgi -b 127.0.0.1:9000
```

php-cgi的特点：

1）php-cgi变更php.ini配置后需重启php-cgi才能让新的配置生效，不可以平滑重启

2）直接杀死php-cgi进程php就不能运行了

### PHP-FPM

PHP-FPM(FastCGI Process Manager)

针对PHP-CGI的不足，PHP-FPM和Spawn-FCGI应运而生，它们的守护进程会平滑重新生成新的子进程。 

1）PHP-FPM使用PHP编写的FastCGI，管理对象是PHP-CGI程序

下载地址：http://php-fpm.org/download

早期的PHP-FPM是作为PHP源码的补丁而使用的，在PHP的5.3.2版本中直接整合到了PHP-FPM分支，目前主流的PHP5.5，PHP5.6，PHP5.7已经集成了该功能（被官方收录）

在配置时使用--enable-fpm参数即可开启PHP-FPM

2）修改php.ini之后，php-cgi进程的确是没办法平滑重启的。php-fpm对此的处理机制是新的worker用新的配置，已经存在的worker处理完手上的活就可以歇着了，通过这种机制来平滑过渡。



本文参考：[关于CGI和FastCGI的理解](https://www.cnblogs.com/tssc/p/10255590.html)

