# 安装

## 一，下载oap-server

```shell
sudo docker pull apache/skywalking-oap-server
```

## 二，下载ui页面

```shell
sudo docker pull apache/skywalking-ui:8.6.0
```

## 三，启动服务

### 3.1 ：默认H2存储

输入以下命令，并耐心待下载。

```shell
sudo docker run --name skywalking -d -p 1234:1234 -p 11800:11800 -p 12800:12800 --restart always apache/skywalking-oap-server 
```

### 3.2：elasticsearch存储

#### 3.2.1：安装ElasticSearch，因为在安装latest版本时失败了，找不到版本信息(Unable to find image 'elasticsearch:latest' locally)，所以这里指定以ElasticSearch 6.72版为例。 

```shell
sudo docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 --restart always -e "discovery.type=single-node" elasticsearch:6.7.2
```

#### 3.2.2:安装 ElasticSearch管理界面elasticsearch-hq　　  

```shell
sudo docker run -d --name elastic-hq -p 5000:5000 --restart always elastichq/elasticsearch-hq
```

#### 3.2.3：输入以下命令，并等待下载。　　

```shell
sudo docker run --name skywalking -d -p 1234:1234 -p 11800:11800 -p 12800:12800 --restart always --link elasticsearch:elasticsearch -e SW_STORAGE=elasticsearch -e SW_STORAGE_ES_CLUSTER_NODES=elasticsearch:9200 apache/skywalking-oap-server 
```

## 四，启动ui页面

```shell
sudo docker run --name skywalking-ui -d -p 8080:8080 --link skywalking:skywalking -e SW_OAP_ADDRESS=skywalking:12800 --restart always apache/skywalking-ui:8.6.0 --security.user.admin.password=admin
```

```shell
sudo docker run --name skywalkingsql -d -p 1234:1234 -p 11800:11800 -p 12800:12800 --restart always --link postgres:postgres -e POSTGRES_PASSWORD=123456 -p 5432:5432 -d postgres apache/skywalking-oap-server 
```

