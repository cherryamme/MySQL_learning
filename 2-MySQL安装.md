<!-- order:2 -->
## MySQL 安装

所有平台的 MySQL 下载地址为： [MySQL 下载](https://www.mysql.com/cn/downloads/). 挑选你需要的 MySQL Community Server 版本及对应的平台。

注意：安装过程我们需要通过开启管理员权限来安装，否则会由于权限不足导致无法安装。

Linux / UNIX 上安装 MySQL
Linux 平台上推荐使用 RPM 包来安装 MySQL ，MySQL AB 提供了以下 RPM 包的下载地址：

- MySQL - MySQL 服务器。如果不是只想连接运行在另一台机器上的 MySQL 服务器，请你选择该选项。
- MySQL-client - MySQL 客户端程序，用于连接并操作 Mysql 服务器。
- MySQL-devel - 库和包含文件，如果你想要编译其它如 Perl 模块等 MySQL 客户端，则需要安装该 RPM 包。
- MySQL-shared - 该软件包包含某些语言和应用程序需要动态装载的共享库(libmysqlclient.so*)，使用 MySQL。
- MySQL-bench - MySQL 数据库服务器的基准和性能测试工具。

### 在windows 安装MySQL

在MySQL官网下载安装包安装后自动打开，可在任务管理器看到MySQL服务进程`mysqld.exe`。

#### 启动服务

- `services.msc`可看到MySQL的服务项，随开机启动；
- 也可通过cmd命令`net start MySQL`启动；停止使用`net stop MySQL`。

#### 登陆命令

```shell
mysql -h hostname -u root -p
```

成功后即可执行SQL语句。

### 在Ubantu安装MySQL

```shell
sudo apt-get install mysql-server
# 在成功安装mysql后，可以直接使用root账户登录，注意这个账户是默认没有密码的。因此为了数据库的安全，需要第一时间给root用户设置密码。
GRANT ALL PRIVILEGES ON *.* TO root@localhost IDENTIFIED BY "<password>";
# 将以上命令中的<password>替换为你要设定的密码即可。设置密码后，如果再以root用户登录就需要输入密码了
```



### 在Mac上安装MySQL

```shell
# 使用brew安装
brew install mysql
# 验证是否安装完成
mysqladmin --version
# 启动mysql服务
mysql.server start
# 检查是否启动mysql
ps -ef | grep mysqld

# 关闭运行中的数据库
mysqladmin -u root -p shutdown

# 更换密码
mysqladmin -u root password "new_password";
# or mysql_secure_installation 
# 使用root账户登陆mysql
mysql -u root -p
```
当输出 `mysql>` 提示符，这说明你已经成功连接到 MySQL 服务器上
如此图：![image-20210923145424721](img_Mysql入门/image-20210923145424721.png)

### Linux系统启动时启动 MySQL

如果你需要在Linux系统启动时启动 MySQL 服务器，你需要在 /etc/rc.local 文件中添加以下命令：

```shell
/etc/init.d/mysqld start
```

同样，你需要将 mysqld 二进制文件添加到 /etc/init.d/ 目录中。
