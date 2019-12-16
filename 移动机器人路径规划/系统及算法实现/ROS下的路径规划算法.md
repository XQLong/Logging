## 总体思路和相关资料

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/12/16/test-1576458333574.png)

[navigation功能包](http://wiki.ros.org/navigation)

[nav_core](http://wiki.ros.org/nav_core)

[classnav__core](http://docs.ros.org/hydro/api/nav_core/html/classnav__core_1_1BaseGlobalPlanner.html)

## 相关概念

- tf坐标变换：
机器人本体和机器人的工作环境中，往往存在大量的组件元素，在机器人设计和机器人应用中都会涉及不同组件的位置和姿态，这就需要引入坐标系以及坐标变换的概念。坐标变换是机器人系统中常用的基础功能，ROS中的坐标变换系统由TF功能包维护。

- tf功能包
TF是一个让用户随时间跟踪多个坐标系的功能包，它使用树型数据结构，根据时间缓冲并维护多个坐标系之间的坐标变换关系，可以帮助开发者在任意时间，在坐标系间完成点、向量等坐标的变换。

- 传感器信息(sensor source)
导航功能需使用来自传感器的信息避开现实环境中的障碍物，它假定这些传感器在ROS上不断发布sensor_msgs/LaserScan消息或者sensor_msgs/PointCloud消息。

- 里程信息(odometry source)
里程计包含两个方面的信息：位姿（位置和转角）和速度（前进速度和转向速度）。导航功能需要使用tf和nav_msgs/Odometry消息发布的里程信息。

- 基座控制器(base controller)
导航功能包假定它可以通过话题"cmd_vel"发布geometry_msgs/Twist类型的消息，这个消息基于机器人的基座坐标系，它传递的是运动命令。这意味着必须有一个节点订阅"cmd_vel"话题， 将该话题上的速度命令(vx, vy, vtheta转换为电机命令(cmd_vel.linear.x, cmd_vel.linear.y, cmd_vel.angular.z)发送给移动基站（机器人）。



## navigation功能包

navigation功能包从里程计和传感器数据流获取信息，并将速度命令发送给移动基站（比如你的机器人）。使用导航功能包集的先决条件是，机器人必须运行ROS，有一个tf变换树，使用正确的ROS消息类型发布传感器数据。而且，我们需要在高层为一个具有一定形状和动力学特点的机器人配置导航功能包集。

### navigation功能包设置

#### 创建功能包

创建一个功能包，保存所有的配置文件和启动文件，包含所有用于实现导航功的所有能所需要的依赖。

```
catkin_create_pkg my_robot_name_2dnav move_base my_tf_configuration_dep my_odom_configuration_dep my_sensor_configuration_dep
```

#### 创建机器人启动配置文件



