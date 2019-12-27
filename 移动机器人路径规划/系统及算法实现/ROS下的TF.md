# tf简单介绍

- tf坐标变换：
机器人本体和机器人的工作环境中，往往存在大量的组件元素，在机器人设计和机器人应用中都会涉及不同组件的位置和姿态，这就需要引入坐标系以及坐标变换的概念。坐标变换是机器人系统中常用的基础功能，ROS中的坐标变换系统由TF功能包维护。

- tf功能包
TF是一个让用户随时间跟踪多个坐标系的功能包，它使用树型数据结构，根据时间缓冲并维护多个坐标系之间的坐标变换关系，可以帮助开发者在任意时间，在坐标系间完成点、向量等坐标的变换。

下面通过一个demo来对其进行感性的认识。

## 安装tutorial节点

```
sudo apt-get install ros-kinetic-ros-tutorials ros-kinetic-geometry-tutorials ros-kinetic-rviz ros-kinetic-rosbash ros-kinetic-rqt-tf-tree
```
## 运行demo

```
roslaunch turtle_tf turtle_tf_demo.launch
```
![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/12/23/1577084808998-1577084809506.png)

将会看到两只乌龟，一只向另一只移动，直到位置重合。选中执行启动命令的终端后可以通过键盘上的方向键来控制中间乌龟的移动，则另外一只会跟随它移动。

## demo原理

这个demo使用TF库创建了三个坐标系，world坐标系，turtle1坐标系，turtle2坐标系。demo使用tf broadcaster发布turtle坐标帧，并使用tf listener来计算坐标帧中的差异，让一个跟随另一个。

## TF Tools

通过tf tools来了解demo底层的运作机制。

### view_frames

view_frames工具创建tf功能包通过ROS广播坐标帧的架构图，

```
rosrun tf view_frames
```
会打印以下信息：
```
Transform Listener initing
Listening to /tf for 5.000000 seconds
Done Listening
dot - Graphviz version 2.16 (Fri Feb  8 12:52:03 UTC 2008)

Detected dot version 2.16
frames.pdf generated
```

查看生成的pdf文件，

```
evince frames.pdf
```

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/12/23/1577094172929-1577094172945.png)

### rqt_tf_tree

rqt_tf_tree是一个运行工具用来将通过ros的树形广播架构可视化，使用如下命令:

```
rosrun rqt_tf_tree rqt_tf_tree
```
或者，
```
rqt &
```
会得到如图所示的结构图，
![title](https://r aw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/12/23/1577099042630-1577099042650.png)

### tf_echo

tf_echo用来生成两个坐标系广播之间的转换报告，命令如：
```
rosrun tf tf_echo [reference_frame] [target_frame]
```
例如对应demo中的两个坐标系，
```
rosrun tf tf_echo turtle1 turtle2
```

### rviz和tf

rviz是一个可以用来检查tf坐标的非常有用的可视化工具，例如在demo中可以使用如下指令：

```
rosrun rviz rviz -d `rospack find turtle_tf`/rviz/turtle_rviz.rviz
```
## 创建一个使用tf功能的应用

以使用python语言为例。
### 创建一个发布者
### 创建一个监听者
### 添加一个坐标系