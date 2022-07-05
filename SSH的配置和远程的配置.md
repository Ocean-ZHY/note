# 配置ubuntu ssh

## 1.在线安装ssh服务

安装ssh-client命令：

```shell
sudo apt-get install openssh-client
```

安装ssh-server命令：

```shell
sudo apt-get install openssh-server
```

## 2.开启服务

```
/etc/init.d/ssh start
```



## 3.修改ssh登录配置

```shell
sudo gedit /etc/ssh/sshd_config
```

- 将PermitRootLogin prohibit-password那一行修改为PermitRootLogin yes

- port 22前面的#去掉

  

## 4.重启ssh服务

```shell
sudo service ssh restart
```

xxxxxxxxxx //Emplyee.javapackage com.example.test01;​public class Employee {​    private Integer id;    private String text;​    public Integer getId() {        return id;    }​    public void setId(Integer id) {        this.id = id;    }​    public String getText() {        return text;    }​    public void setText(String text) {        this.text = text;    }}java

查看虚拟机的IP地址：ifconfig

如果找不到则使用如下命令安装工具包

```shell
sudo apt install net-tools           //使用apt源安装net-tools工具包
```

![](.\image\image-20220223202816465.png)

然后在termius中输入IP地址和ssh相关信息

![](.\image\image-20220223202959511.png)