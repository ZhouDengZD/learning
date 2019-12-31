
mongodb内置角色主要分为以下几类：
1.数据库用户角色
2.数据库管理角色
3.集群管理角色
4.备份和恢复角色
5.全数据库级角色
6.超级用户角色
7.内部角色



权限配置首先需要启用访问控制：在mongodb配置数据（在安装目录的\bin下）里做如下修改即可。
![](https://ftp.bmp.ovh/imgs/2019/12/6abc29ef813649da.png)

这样就开启了访问控制，访问数据库的时候就会进行权限验证。


创建具有某种权限的用户，以超级管理员root为例：
- use admin
- db.createUser({user:'admin',pwd:'adminadmin',roles:[{role:'root',db:'admin'}]})

连接数据库：mongo admin（数据库名） -u admin（账户名） -p adminadmin（密码）