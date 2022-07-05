# 安装

## 由于apt官方库里的docker版本可能比较旧，所以先卸载可能存在的旧版本：

```shell
sudo apt-get remove docker docker-engine docker-ce docker.io
```

## 更新apt包索引：

```shell
sudo apt-get update
```

## 安装以下包以使apt可以通过HTTPS使用存储库（repository）：

```shell
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
```

## 添加Docker官方的GPG密钥：

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

## 使用下面的命令来设置stable存储库：

```shell
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

## 再更新一下apt包索引：

```shell
sudo apt-get update
```

## 安装最新版本的Docker CE：

```shell
sudo apt-get install -y docker-ce 
```

## 验证docker

查看docker服务是否启动：

```shell
systemctl status docker
```

![image-20220413135047750](.\image\image-20220413135047750.png)

- 若未启动，则启动docker服务：

```ruby
sudo systemctl start docker
```

- 经典的hello world：

```ruby
sudo docker run hello-world
```

## 查看docker运行的所有进程

```shell
sudo docker ps -a
```

![image-docer2](.\image\image-docer2.png)