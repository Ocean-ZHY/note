# ubuntu安装postgresql

https://www.postgresql.org/download/linux/ubuntu/

```shell
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql
```

```shell
$ sudo su - postgres //进入sql编辑
$ psql
postgres=# CREATE USER yy WITH PASSWORD 'yy';
CREATE ROLE
postgres=# CREATE DATABASE blogbase;
CREATE DATABASE
postgres=# GRANT ALL PRIVILEGES ON DATABASE blogbase to yy;
GRANT
修改用户密码

alter user 用户名 with password '新密码';
ALTER ROLE
登录数据库

psql -U yy -d blogbase -h 127.0.0.1
-U:指定用户，-d:指定数据库，-h:指定服务器,如有端口用 -p 指定

执行上述命令后，如果出现例如 "Password for user yy: " 这样的语句让输入密码，输入密码后即登录成功

重命名数据库

alter database blogbase rename to blogbase1;
服务

# 查看状态
sudo /etc/init.d/postgresql status

# 启动
sudo /etc/init.d/postgresql start

# 停止
sudo /etc/init.d/postgresql stop

# 重启
sudo /etc/init.d/postgresql restart
```

# docker安装postgresql

```dockerfile
docker pull postgres
```

```dockerfile
docker run --name some-postgres -e POSTGRES_PASSWORD=123456 -p 5432:5432 -d postgres
```

