---

title: PostgreSQL的安装和配置
categories: [计算机, 数据库]
tags: [PostgreSQL]
authors: [badbadyi]

---

*本文运行环境基于 WSL2， Linux 发行版为 Ubuntu 22.04.3 LTS*

*安装版本为 PostgreSQL 16*

## 安装

更新包索引并安装必需依赖

```sh
sudo apt update
sudo apt install gnupg2 wget vim
```

添加 PostgreSQL 16 仓库配置

```sh
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

导入仓库签名密钥

```sh
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

再次更新索引

```sh
sudo apt update
```

**执行如下指令来安装 PostgreSQL 16 和 contrib包（包含一些实用程序）**

```sh
sudo apt install postgresql-16 postgresql-contrib-16
```

安装成功后，通常情况下数据库会直接启动

*数据库还会为你自动为你创建一个名为 `postgres` 的 Linux 用户*

*同时还会有一个同名的数据库角色（超级角色）和数据库*

确认 PostgreSQL 的运行状态

```sh
sudo service postgresql status
```

如果数据库服务器没有启动，可以尝试如下指令

```sh
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

确认 PostgreSQL 的版本

```sh
psql --version
```

得到类似如下的返回信息

```
psql (PostgreSQL) 16.1 (Ubuntu 16.1-1.pgdg22.04+1)
```

## 连接数据库

通过 `psql` 命令来连接到 PostgresSQL

不带参数使用的 `psql` 会以当前的 **Linux 用户名**作为**数据库角色名**来尝试连接到**同名的数据库**

所以你可以通过切换到名为 `postgres` 的 Linux 用户再执行 `psql` 来连接到 `postgres` 数据库

1. 切换到 `postgres` 用户

   *该用户在安装数据库时被自动添加*

   ```sh
   sudo su postgres
   ```

2. 直接执行 `psql` 来连接到 `postgres` 数据库

   ```
   postgres@yi-laptop:~$ psql
   psql (16.1 (Ubuntu 16.1-1.pgdg22.04+1))
   Type "help" for help.
   
   postgres=#
   ```

或者通过如下命令直接连接到数据库

```sh
sudo -u postgres psql
```

其中 `sudo -u postgres` 即以 `postgres` Linux 用户的身份来执行

*确保你的当前用户有 `sudo` 权限*

## 创建新的数据库和角色

*对于 PostgreSQL，数据库角色和数据库用户是一个概念*

通过如下命令创建一个新的数据库角色，以及其登录密码

```sql
CREATE USER <username> WITH PASSWORD '<password_here>' ;
```

将 `<username>` 替换为你需要的角色名，将 `<password_here>` 替换为该角色的登录密码，**注意不要省略单引号 `'`**

*创建一个与你最常用的 Linux 用户名同名的数据库角色名会比较省事，免去了再去另外创建 Linux 用户名的麻烦*

通过如下命令新建一个数据库，并指定它的所有者（例如，上面指令的 `<username>`）

```sql
CREATE DATABASE <database_name> OWNER <username>;
```

将 `<database_name>` 替换为你需要的数据库名，将 `<username>` 替换为该数据库的拥有者

*创建一个与 `<username>` 相同名字的数据库可以让你在通过 `psql` 连接时少输入一个带数据库名的参数*

举例

```sql
CREATE USER arthur WITH PASSWORD '******'; -- 创建带密码的数据库角色 arthur
CREATE DATABASE mydb OWNER arthur; -- 创建数据库 mydb 并指定拥有者 arthur
\q -- 退出
```

如果 Linux 用户名为如上例子的 `arthur`，可以直接执行如下命令来连接到数据库 `mydb`

```sh
psql -d mydb
```

## 逻辑

对于 PostgreSQL 数据库，数据库角色与 Linux 用户是对等的

特定的 Linux 用户代表了其对应的数据库角色（即同名）

PostgreSQL 安装时，数据库会自带初始数据库 `postgre`，以及超级用户 `postgre`（拥有最高权限）

除此之外，还会自动创建一个新的 Linux 用户 `postgre`

## 图形化工具

[pgAdmin - PostgreSQL Tools](https://www.pgadmin.org/)
