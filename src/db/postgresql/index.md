# PostgreSQL

The World's Most Advanced Open Source Relational Database

- [official site](https://www.postgresql.org/)
- [Docs](https://www.postgresql.org/docs/)
- [Downloading and Installing PostgreSQL](https://www.postgresql.org/download/)

## command

### 常用语法

```text
# 切换/连接数据库
create database jeelowcode;

# 列出所有数据库
\l

# 切换/连接数据库
\c jeelowcode

# 删除数据库
drop database jeelowcode;

# 查看所有表
\dt

# 查看表结构
\d users

# 删除表
DROP TABLE users;

CREATE USER username WITH PASSWORD 'mypassword';	创建新用户
GRANT ALL PRIVILEGES ON DATABASE mydb TO username;	授予用户数据库权限
GRANT SELECT, INSERT ON TABLE users TO username;	授予用户表操作权限
```

### pg_ctl

```text
pg_ctl --help

pg_ctl 是一个用于初始化、启动、停止或控制PostgreSQL服务器的工具.

使用方法:
  pg_ctl init[db]   [-D 数据目录] [-s] [-o 选项]
  pg_ctl start      [-D 数据目录] [-l 文件名] [-W] [-t 秒数] [-s]
                    [-o 选项] [-p 路径] [-c]
  pg_ctl stop       [-D 数据目录] [-m SHUTDOWN-MODE] [-W] [-t 秒数] [-s]
  pg_ctl restart    [-D 数据目录] [-m SHUTDOWN-MODE] [-W] [-t 秒数] [-s]
                    [-o 选项] [-c]
  pg_ctl reload     [-D 数据目录] [-s]
  pg_ctl status     [-D 数据目录]
  pg_ctl promote    [-D 数据目录] [-W] [-t 秒数] [-s]
  pg_ctl logrotate  [-D 数据目录] [-s]
  pg_ctl kill       信号名称 进程号
  pg_ctl register   [-D 数据目录] [-N 服务名称] [-U 用户名] [-P 口令]
                    [-S 启动类型] [-e 源] [-W] [-t 秒数] [-s] [-o 选项]
  pg_ctl unregister [-N 服务名称]

普通选项:
  -D, --pgdata=数据目录  数据库存储区域的位置
  -e SOURCE              当作为一个服务运行时要记录的事件的来源
  -s, --silent           只打印错误信息, 没有其他信息
  -t, --timeout=SECS     当使用-w 选项时需要等待的秒数
  -V, --version          输出版本信息, 然后退出
  -w, --wait             等待直到操作完成(默认)
  -W, --no-wait          不用等待操作完成
  -?, --help             显示此帮助, 然后退出
如果省略了 -D 选项, 将使用 PGDATA 环境变量.

启动或重启的选项:
  -c, --core-files       在这种平台上不可用
  -l, --log=FILENAME     写入 (或追加) 服务器日志到文件FILENAME
  -o, --options=OPTIONS  传递给postgres的命令行选项
                         (PostgreSQL 服务器执行文件)或initdb
  -p PATH-TO-POSTMASTER  正常情况不必要

停止或重启的选项:
  -m, --mode=MODE        可以是 "smart", "fast", 或者 "immediate"

关闭模式有如下几种:
  smart       所有客户端断开连接后退出
  fast        直接退出, 正确的关闭（默认）
  immediate   不完全的关闭退出; 重启后恢复

允许关闭的信号名称:
  ABRT HUP INT KILL QUIT TERM USR1 USR2

注册或注销的选项:
  -N 服务名称     注册到 PostgreSQL 服务器的服务名称
  -P 口令         注册到 PostgreSQL 服务器帐户的口令
  -U 用户名       注册到 PostgreSQL 服务器帐户的用户名
  -S START-TYPE   注册到PostgreSQL服务器的服务启动类型

启动类型有:
  auto       在系统启动时自动启动服务(默认选项)
  demand     按需启动服务
```

### psql

```text
psql
命令	功能
\?	查看所有psql命令帮助
\h	查看SQL命令帮助（如 \h CREATE TABLE）
\l	列出所有数据库
\c dbname	连接到另一个数据库
\dt	列出当前数据库的所有表
\d tablename	查看指定表的结构
\x	切换扩展显示模式（查看宽表时有用）
\q	退出psql

PS C:\Users\PC> psql --help
psql是PostgreSQL 的交互式客户端工具。
使用方法:
  psql [选项]... [数据库名称 [用户名称]]
通用选项:
  -c, --command=命令       执行单一命令(SQL或内部指令)然后结束
  -d, --dbname=DBNAME      database name to connect to
  -f, --file=文件名        从文件中执行命令然后退出
  -l, --list               列出所有可用的数据库,然后退出
  -v, --set=, --variable=NAME=VALUE
                           设置psql变量NAME为VALUE
                           (例如，-v ON_ERROR_STOP=1)
  -V, --version            输出版本信息, 然后退出
  -X, --no-psqlrc          不读取启动文档(~/.psqlrc)
  -1 ("one"), --single-transaction
                           作为一个单一事务来执行命令文件(如果是非交互型的)
  -?, --help[=options]     显示此帮助，然后退出
      --help=commands      列出反斜线命令，然后退出
      --help=variables     列出特殊变量，然后退出

输入和输出选项：
  -a, --echo-all           显示所有来自于脚本的输入
  -b, --echo-errors        回显失败的命令
  -e, --echo-queries       显示发送给服务器的命令
  -E, --echo-hidden        显示内部命令产生的查询
  -L, --log-file=文件名    将会话日志写入文件
  -n, --no-readline        禁用增强命令行编辑功能(readline)
  -o, --output=FILENAME    将查询结果写入文件(或 |管道)
  -q, --quiet              以沉默模式运行(不显示消息，只有查询结果)
  -s, --single-step        单步模式 (确认每个查询)
  -S, --single-line        单行模式 (一行就是一条 SQL 命令)

输出格式选项 :
  -A, --no-align           使用非对齐表格输出模式
      --csv                CSV（逗号分隔值）表输出模式
  -F, --field-separator=STRING
                           为字段设置分隔符,用于不整齐的输出(默认："|")
  -H, --html               HTML 表格输出模式
  -P, --pset=变量[=参数]   设置将变量打印到参数的选项(查阅 \pset 命令)
  -R, --record-separator=STRING
                           为不整齐的输出设置字录的分隔符(默认：换行符号)
  -t, --tuples-only        只打印记录i
  -T, --table-attr=文本    设定 HTML 表格标记属性（例如,宽度,边界)
  -x, --expanded           打开扩展表格输出
  -z, --field-separator-zero
                           为不整齐的输出设置字段分隔符为字节0
  -0, --record-separator-zero
                           为不整齐的输出设置记录分隔符为字节0

联接选项:
  -h, --host=HOSTNAME      database server host or socket directory
  -p, --port=PORT          database server port
  -U, --username=USERNAME  database user name
  -w, --no-password        永远不提示输入口令
  -W, --password           强制口令提示 (自动)
```

## Win11 Installing

### 未配置环境变量

```powershell
# 初始化 pgsql 数据库
# -D 数据目录 -A 密码认证方式 -U 超级管理员 -W 强制要求设置密码
.\initdb.exe -D D:\pgsql_data -A scram-sha-256 -U postgres -W

# 启动 pgsql
# -D 数据目录 -l 指定日志文件
.\pg_ctl.exe -D D:\pgsql_data -l pgsql.log  start

# 关闭 pgsql
# -D 数据目录 -l 指定日志文件
.\pg_ctl.exe -D D:\pgsql_data -l pgsql.log  stop

# 重启 pgsql
# -D 数据目录 -l 指定日志文件
.\pg_ctl.exe -D D:\pgsql_data -l pgsql.log  restart

# 到此时 仅能本地连接
.\psql.exe -h localhost -U postgres

# 配置文件 D:\pgsql_data\pg_hba.conf
# 测试环境 在配置文件末尾 添加一行规则，允许所有远程IP连接
host    all             all             0.0.0.0/0               scram-sha-256
# 生产环境 在配置文件末尾 添加一行规则，仅允许指定网段IP连接
host    all             all             192.168.1.0/24          scram-sha-256
```

### 配置环境变量 Path

**_C:\Program Files\pgsql\bin_** \
**_C:\Program Files\pgsql\pgAdmin 4\runtime_**

### 注册服务

```powershell
# 将 pgsql 注册为 win11 服务
pg_ctl register -N "PostgreSQL" -D D:\pgsql_data -S demand  -w

# 查看服务状态
Get-Service PostgreSQL
# 启动服务
Start-Service PostgreSQL
# 停止服务
Stop-Service PostgreSQL
# 重启服务
Restart-Service PostgreSQL
# 删除服务
sc.exe delete PostgreSQL
```
