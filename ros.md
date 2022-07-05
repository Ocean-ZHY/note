## **ROS2系统安装**

接下来，我们就可以把ROS2安装到Ubuntu系统中了。安装步骤如下：

### 1. 设置编码

```shell
$ sudo apt update && sudo apt install locales
$ sudo locale-gen en_US en_US.UTF-8
$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 
$ export LANG=en_US.UTF-8
```

### 2. 添加源

```shell
$ sudo apt update && sudo apt install curl gnupg lsb-release 
$ sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg 
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

> 如遇报错“**Failed to connect to raw.githubusercontent.com**”，可参考https://www.guyuehome.com/37844
>
> 解决办法如下：
>
> 1. 登录网站：[https://www.ipaddress.com](https://www.ipaddress.com/)
>
> 2. 在打开的网站中将“raw.githubusercontent.com”复制到查询栏中进行搜索，可以看到域名对应的IP地址信息
>
> [![img](.\image\20220430_77495.png)](https://www.guyuehome.com//Uploads/Editor/202204/20220430_77495.png)
>
> 3. 将搜索结果中展示的Ip地址和域名拷贝系统hosts文件中：
>
> ```bash
> $ sudo vi /etc/hosts
> ```
>
> [![img](.\image\20220430_74707.png)](https://www.guyuehome.com//Uploads/Editor/202204/20220430_74707.png)
>
> 4.保存退出后，就可以正常使用了。
>
> ![img](.\image\20220430_20846.png)

### 3. 安装ROS2

```shell
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install ros-humble-desktop
```

如遇部分包资源下载失败，可通过换源解决，参考https://blog.csdn.net/weixin_44120025/article/details/122317808?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-122317808-blog-111239986.pc_relevant_antiscanv2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-122317808-blog-111239986.pc_relevant_antiscanv2&utm_relevant_index=5

![image-20220616134325449](.\image\image-20220616134325449.png)

解决办法如下：

![在这里插入图片描述](.\image\0bf257fe6a484364b5d43f9bdafbc2b4.png)

![在这里插入图片描述](.\image\3e2b692b056e49c287fe0a7aca61016f.png)

### 4. 设置环境变量

```shell
$ source /opt/ros/humble/setup.bash
$ echo " source /opt/ros/humble/setup.bash" >> ~/.bashrc 
$ sudo apt install python3-colcon-common-extensions
```

至此，ROS2就已经在系统中安装好了。

    CATKIN_BINARY_DIR="/home/catkin/build" \
    "/usr/bin/python" \
    "/home/catkin/setup.py" \

如遇错误“catkin: command not found”

#### 解决方案1

```shell
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
$ wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
$ sudo apt-get install python-catkin-tools
```

如果还不能解决，尝试下面方案。

#### 解决方案2：

```shell
$ git clone https://github.com/ros/catkin.git
$ cd catkin
$ git branch melodic-devel
$ mkdir build
$ cd build
$ cmake ..
$ make -j8
$ sudo make install
$ cd ..
$ sudo python3 setup.py install
```

```shell
如遇"git clone:Failed to connect to github.com port 443: Connection refused"
解决方法：
$ sudo gedit /etc/hosts
```

![在这里插入图片描述](.\image\watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAZGFz55m9,size_19,color_FFFFFF,t_70,g_se,x_16)

```shell
如遇"fatal: could not create work tree dir ‘xxxx’: Permission denied"
$ cd ../    （回退到当前目录的上一级）
$ sudo chmod o+w dirname  （dirname为当前目录的名字）
```



#### 解决方案3：

```shell
$ sudo apt-get install python3-pip
$ sudo pip3 install -U catkin_tools
```



![image-20220620162830623](.\image\image-20220620162830623.png)

```shell
"观察报错，share和lib应该是包含关系（报错图中红色虚线部分），但是我的目录中share和lib是并列关系。把share移动到lib文件夹下"
$ cd /usr/local/
$ sudo mv share /usr/local/lib/
```

![image-20220620163151778](.\image\image-20220620163151778.png)

再次输入"catkin_make",若出现如下错误，则是权限不够

![image-20220620163405536](.\image\image-20220620163405536.png)

```shell
"权限不够"
$ sudo catkin_make
```

![image-20220620163913521](.\image\image-20220620163913521.png)

https://www.cnblogs.com/tensorrt/p/14845290.html