# 在mac中如何使用mysql
* mysql安装
* mysql配置
* mysql界面管理工具
* mysql使用总结

## mysql的安装
利用 homebrew 进行安装。
```
 brew install mysql

```


## mysql配置
需要使用命令来配置，一般是用来做服务的启动、停止、重启用
``` mysql
    We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

To connect run:
    mysql -uroot

To have launchd start mysql now and restart at login:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  mysql.server start
```

## mysql界面管理工具
使用两个GUI工具试试：workbench、sequel pro
因workbench是官方的，所以先使用它。

## mysql使用总结
因还在摸索中，所以暂时无法补充