cron是一个linux下 的定时执行工具，可以在无需人工干预的情况下运行作业。
　　service crond start  //启动服务
　　service crond stop   //关闭服务
　　service crond restart //重启服务
　　service crond reload  //重新载入配置
　　service crond status  //查看服务状态 

crontab 参数： 

-l 显示当前的crontab。 
-r 删除当前的crontab文件。 
-e 编辑当前用户的crontab文件。当结束编辑离开时，编辑后的文件将自动安装。 

当修改crontab文件之后，要重启服务使修改生效。

定时执行php脚本参考[这里](https://blog.csdn.net/qq_28285347/article/details/78837735###)。

