1. CI项目文件夹safe-uni-api放到开发机的某个路径下，如/home/liyunzeng/phpcode/safe-uni-api

2. CI项目的config.php设置`$config['base_url']` ,这个主要用来补全CI项目中发起请求时没有写完整的url

3. config.php中设置`$config['index_page'] = '';`在输入url时就可以省略url中`/index.php`部分。

4. 接下来配置nginx，nginx/conf/nginx.conf（nginx默认配置文件）中有

   ![image-20200806155815357](https://github.com/YunzengLee/my_pictures/blob/master/md_pic/image-20200806155815357.png?raw=true)

   server{}同级下有`include include/*.conf`，说明在`/nginx/conf/include`下所有`.conf`文件将被自动读取。

5. 在include目录下新建一个`liyunzeng-safe-uni-api.conf`文件，内容如下：

   ![image-20200806160155787](https://github.com/YunzengLee/my_pictures/blob/master/md_pic/image-20200806160155787.png?raw=true)

6. 注意server_name的设置，可以自由设置，但最好和CI项目config.php中一致。

   fastcgi_pass 指向的端口是9001，是因为之前安装php5，设置了php5的php-fpm服务监听9001端口。

7. 接下来重启nginx服务，`sudo service nginx reload`

8. 然后需要映射server_name，修改开发机的`/etc/hosts`，最后一行加上`127.0.0.1   tools.qa.safe.360.cn`

9. 然后在本地主机的浏览器上访问项目，需要下载SwithHosts（用来快捷切换和管理hosts），设置开发机ip和server_name。

   ![image-20200806160714741](https://github.com/YunzengLee/my_pictures/blob/master/md_pic/image-20200806160714741.png?raw=true)

10. 然后在本地浏览器中输入`tools.qa.safe.360.cn/controller/method`就可以访问项目了。

11. 可以使用网络驱动映射器将开发机的某个文件夹映射为本地磁盘，这样就可以使用本地IDE实时改动开发机上的项目代码，看上去就像改动本地的代码一样。

    ![image-20200806161009852](https://github.com/YunzengLee/my_pictures/blob/master/md_pic/image-20200806161009852.png?raw=true)