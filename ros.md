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









## **ROS1系统安装**

#### 1.配置ubuntu的软件和更新

配置ubuntu的软件和更新，允许安装不经认证的软件。

首先打开“软件和更新”对话框，具体可以在 Ubuntu 搜索按钮中搜索。

打开后按照下图进行配置（确保勾选了"restricted"， "universe，" 和 "multiverse."）

![00ROS安装之ubuntu准备](.\image\00ROS安装之ubuntu准备.png)



#### 2.设置安装源

官方默认安装源:

```shell
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

或来自国内清华的安装源

```shell
$ sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
```

或来自国内中科大的安装源

```shell
$ sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
```

PS:

1. 回车后,可能需要输入管理员密码
2. 建议使用国内资源，安装速度更快。

#### 3.设置key

```shell
$ sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

#### 4.安装

首先需要更新 apt(以前是 apt-get, 官方建议使用 apt 而非 apt-get),apt 是用于从互联网仓库搜索、安装、升级、卸载软件或操作系统的工具。

```shell
$ sudo apt update
```

等待...

然后，再安装所需类型的 ROS:ROS 多个类型:**Desktop-Full**、**Desktop**、**ROS-Base**。这里介绍较为常用的Desktop-Full(官方推荐)安装: ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators, navigation and 2D/3D perception

```shell
$ sudo apt install ros-noetic-desktop-full
```

等待......(比较耗时)

友情提示: 由于网络原因,导致连接超时，可能会安装失败，如下所示:![09\_安装异常](.\image\09_安装异常.PNG)可以多次重复调用 更新 和 安装命令，直至成功。

#### 5.配置环境变量

配置环境变量，方便在任意 终端中使用 ROS。

```shell
$ echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
$ source ~/.bashrc
```

------

#### 卸载

如果需要卸载ROS可以调用如下命令:

```shell
$ sudo apt remove ros-noetic-*
```

注意: 在 ROS 版本 noetic 中无需构建软件包的依赖关系，没有`rosdep`的相关安装与配置。



#### 6.安装构建依赖

在 noetic 最初发布时，和其他历史版本稍有差异的是:没有安装构建依赖这一步骤。随着 noetic 不断完善，官方补齐了这一操作。

首先安装构建依赖的相关工具

```shell
$ sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
```

ROS中使用许多工具前，要求需要初始化rosdep(可以安装系统依赖) -- 上一步实现已经安装过了。

```shell
$ sudo apt install python3-rosdep
```

初始化rosdep

```shell
$ sudo rosdep init
$ rosdep update
```

如果一切顺利的话，rosdep 初始化与更新的打印结果如下:

![img](.\image\rosdep正常初始化.PNG)

![img](.\image\rosdep正常更新.PNG)

------

但是，在 rosdep 初始化时，多半会抛出异常。

![image-20220705220658007](.\image\image-20220705220658007.png)

在终端输入：

```shell
$ sudo apt-get install python3-pip
$ sudo pip3 install 6-rosdep
$ sudo 6-rosdep
```

#### 7.测试 ROS

ROS 内置了一些小程序，可以通过运行这些小程序以检测 ROS 环境是否可以正常运行

1. 首先启动三个命令行(ctrl + alt + T)
2. 命令行1键入:**roscore**
3. 命令行2键入:**rosrun turtlesim turtlesim_node**(此时会弹出图形化界面)
4. 命令行3键入:**rosrun turtlesim turtle_teleop_key**(在3中可以通过上下左右控制2中乌龟的运动)

最终结果如下所示:

![01ROS环境测试](.\image\01ROS环境测试.png)注意：光标必须聚焦在键盘控制窗口，否则无法控制乌龟运动。







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

![在这里插入图片描述](.\image\_storage_emulated_0_Download_WeiXin_watermark,type_d)

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