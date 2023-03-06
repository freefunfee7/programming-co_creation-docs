# MySQL 环境准备

## 安装

最新的 MySQL 服务程序可以在[官网下载](https://dev.mysql.com/downloads/installer/)，官网也提供了针对不同操作系统的[安装指引](https://dev.mysql.com/doc/refman/8.0/en/installing.html)。

如果已经按照我们提供的[指南](x1-setup.md)配置好自己机器的编程环境的话，就可以用 [Homebrew](https://brew.sh/)（macOS 下）或 [Scoop](https://scoop.sh/)（Windows 下）来安装。比较推荐这个办法。

### Windows

1. 在 ConEMU 中打开一个 PowerShell (Admin) 会话窗口（即管理员权限的命令行界面，如果有安全提示选择允许运行），然后执行下面的命令：

```powershell
scoop bucket add versions
scoop install mysql57
```

2. 打开 `C:\Users\trust\apps\mysql57\current\my.ini` 文件（如果不存在就创建一个），文件内容应该是这样的：

```ini
[client]
# Which port the client should use as a default.
# this means if you're connecting to the local machine, then you can omit 
# the --port parameter
port=3306

[mysqld]
# Which port you're using. You'll need to write 
# mysql --port=3360 if you're specifying the port
port=3306
# These two have to do with size of data allowed in the system
key_buffer_size=16M
max_allowed_packet=8M

[mysqldump]
# The style of mysqldump to use as default
quick
```

3. 回到前面的 PowerShell (Admin) 窗口，运行：

```powershell
mysqld --install
```

4. 在 Windows 搜索栏输入 Services 找到并运行 Services 管理程序，如果上一步没有问题，打开的列表中应该有 MySQL 一项，右键选择启动（Start）即可。

> 在较老的 Windows 上没有 Services 管理程序，可以打开 Control Panel（控制面板），找到 Administration Tools（管理工具）里面的 Services（服务）。

### macOS

在命令行界面运行下面的命令即可：

```shell
brew install mysql@5.7
brew services start mysql@5.7
```

## 初始配置

当 MySQL Server 运行起来之后，就可以通过命令行工具 `mysql` 来操作，下面的操作无论在 Windows 还是 macOS 下都是一样的。

在命令行下输入 `mysql -uroot` 进入 MySQL 的 REPL：

```shell
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 32
Server version: 5.7.26 Homebrew

Copyright (c) 2000, 2019, Oracle ...

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

在 `mysql>` 提示符之后可以输入任何 MySQL 支持的 SQL 语句（用半角分号 `;` 结尾）。

### 修改 root 密码

注意我们目前是以 `root` 用户的身份登录到 MySQL 服务（命令行里的 `-u` 就是指定登录用户的参数），这是最大权限的用户，可以做任何事情，而且初始没有密码。这是个很不安全的状态，我们首先要改变这个状态。

在 `mysql>` 提示符之后输入（注意将 `mypass` 改为你选择的 `root` 用户密码）：

```sql
SET PASSWORD FOR root@localhost = PASSWORD('mypass');
```

回车运行，`root` 用户的密码已经改成上面设置的，然后可以输入 `EXIT` 退出 MySQL 的 REPL，再次运行 `mysql -uroot` 将无法进入，会有一个 `Access denied` 的错误。

在命令行运行（`-p` 参数表示用户有密码，希望在提示后输入）：

```shell
> mysql -uroot -p
Enter password: *******
```

输入密码后成功进入 REPL。

### 创建数据库和对应用户

以下操作都以 `root` 用户身份在 `mysql` 的提示符后输入执行。

```sql
mysql> CREATE DATABASE demo;
Query OK, 1 row affected (0.01 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| demo               |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.02 sec)

mysql> USE demo
Database changed
mysql> SHOW TABLES;
Empty set (0.01 sec)
```

如上所示，创建了一个新的数据库 `demo`，其中还没有任何表。下面来创建一个用户，并赋予该用户对 `demo` 数据库的完整操作权限（但不能访问 MySQL 服务中的其他数据）。

```sql
mysql> CREATE USER learn@localhost IDENTIFIED BY 'demo';
Query OK, 0 rows affected (0.02 sec)

mysql> GRANT ALL ON demo.* TO learn@localhost;
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.01 sec)
```

上面第一条命令创建了一个叫 `learn` 的用户，限制其只能从本地（`localhost`）登录，并设置其密码为 `demo`；第二条命令则赋予该用户对 `demo` 数据库的完整权限（用户缺省是什么权限都没有的）；最后一条命令是要求服务器刷新权限库，令前面的设置生效。

现在可以退出 REPL，然后尝试用 `mysql -ulearn -p` 来以 `learn` 身份登录，输入密码后可以进入 REPL 界面，然后可以试试看我们能做什么。

```sql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| demo               |
+--------------------+
2 rows in set (0.00 sec)

mysql> use demo
Database changed
mysql> SHOW TABLES;
Empty set (0.00 sec)
                                                                                                              
mysql> CREATE TABLE users (id INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY, username VARCHAR(255), firstname VARCHAR(255), lastname VARCHAR(255));                                                                                                                 
Query OK, 0 rows affected (0.04 sec)                                                                                             
                                                                                                                                 
mysql> SHOW TABLES;                                                                                                              
+----------------+                                                                                                               
| Tables_in_demo |                                                                                                               
+----------------+                                                                                                               
| users          |                                                                                                               
+----------------+                                                                                                               
1 row in set (0.00 sec)                                                                                                          
                                                                                                                                 
mysql> DESC users;                                                                                                               
+-----------+--------------+------+-----+---------+----------------+                                                             
| Field     | Type         | Null | Key | Default | Extra          |                                                             
+-----------+--------------+------+-----+---------+----------------+                                                             
| id        | int(11)      | NO   | PRI | NULL    | auto_increment |                                                             
| username  | varchar(255) | YES  |     | NULL    |                |                                                             
| firstname | varchar(255) | YES  |     | NULL    |                |                                                             
| lastname  | varchar(255) | YES  |     | NULL    |                |                                                             
+-----------+--------------+------+-----+---------+----------------+                                                             
4 rows in set (0.01 sec)

mysql> DROP TABLE users;
Query OK, 0 rows affected (0.01 sec) 
```

可以看到，`learn` 只能访问自己有权限的数据库，并且在 `demo` 数据库内拥有创建和删除表的权限。

至此 MySQL 环境配置完毕。

## 准备学习用数据库

最后我们来准备下在学习中我们会用到的数据库。这部分是可选的，如果没有完成，完全可以用自己创建的简单数据来测试，不过完成下面的操作也是对环境熟悉的过程，你完全可以试试，很多编程的经验都是折腾这些环境和数据得到的。

### 准备数据文件

首先从 [Kagger](https://www.kaggle.com/karangadiya/fifa19) 下载数据，下载回来是一个名为 `fifa19.zip` 的压缩包，解压之后是一个名为 `data.csv` 的 *CSV* 格式数据文件，将其改名为 `fifa19.csv`。

因为数据文件 `fifa19.csv` 的编码格式和我们下面用的工具不兼容，所以需要做一点处理，用 Visual Studio Code 打开这个文件，点击右下角的编码栏 `UTF-8 with BOM`：

<img src="assets/csv-data-in-vscode.png">

点击上图右下角红框标出的栏，窗口上方会弹出菜单，依次选择 *Save with Encoding* 和 *UTF-8*，然后可以关闭 Visual Studio Code。

### 数据导入

我们使用官方的 GUI 工具来完成这个导入。

首先从 MySQL 官方网站下载 [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) 并运行下载好的安装包（名字一般是 `mysql-workbench-community-x.x.xx-winx64.msi`）。

打开安装好的 MySQL Workbench，如果前面 MySQL 服务安装和运行无误，这里 Workbench 会检测到本地运行的服务，并显示在启动界面，点击之就可以连接开始管理本地的数据库：

<img src="assets/mysql-workbench.png">

如果前面的初始配置无误，在打开的界面左侧选择 *Schemas* 就可以看到 `demo` 数据库，右键选择 `demo` 数据库然后选择 *Table Data Import Wizard* 打开数据导入向导：

<img src="assets/mysql-data-import.png">

按照向导一步步操作：
1. 浏览并选择刚才我们解压并修改过的 `fifa19.csv`，点击 *Next >*；
2. 选择 *Create new table*，下拉框里选择数据库 `demo`，表名输入 `players`，点击 *Next >*；
3. 在 *Columns* 下面去掉第一行 `MyUnknownColumn` 那一行的选择框，点击 *Next >*；
4. 点击 *Next >* 开始导入数据，这要一会儿。

完成后在主界面 `demo` 数据库下面可以看到新建的 `players` 表，如果没有可以右键点击 `demo` 选择 *Refresh All*。

右键点击 `players` 表选择 *Select Rows - Limit 1000* 可以查询显示表的前 1000 行，从而确认导入成功。
