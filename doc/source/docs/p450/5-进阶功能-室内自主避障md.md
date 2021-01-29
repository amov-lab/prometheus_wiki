# 进阶功能-室内自主避障

## 硬件
T265<br/>
思岚激光雷达A2<br/>
P200无人机<br/>
TX2<br/>
## 软件
普罗米修斯<br/>

## 参数设置

## 实际操作
打开两个终端依次输入一下两个命令：
```
roslaunch prometheus_experiment prometheus_vio_rplidar_astar.launch
```
```
roslaunch prometheus_experiment prometheus_vio_rplidar_astar_groundstation.launch
```
第一条命令运行后会看到无人机上的激光雷达开始转起来

可在弹出的rviz窗口查看激光雷达的扫描效果
![](https://img-blog.csdnimg.cn/20201218085907850.png)


在下面终端输入0 并敲回车，选择command input control也就是命令输入控制
![](https://img-blog.csdnimg.cn/20201218083958833.png)<br/>
再输入1并敲回车，也就是选择Takeoff起飞。
![](https://img-blog.csdnimg.cn/2020121808405157.png)

再遥控器SWC切定点，左边单杆内八解锁，SWD切offboard，切到offboard模式后无人机会自动起飞到起飞点上方设定高度保持悬停（默认0.5米）。

然后在如下窗口选择是输入2D的位置还是输入3D的位置（注意这个操作是在起飞之后），如果是输入2D位置，那么高度z是默认和当前悬停高度一致。
![](https://img-blog.csdnimg.cn/20201218083323815.png)<br/>
输入0并敲回车，选择输入2D点坐标<br/>
![](https://img-blog.csdnimg.cn/20201218084744396.png)<br/>

敲回车后会显示如下，此时可以输入目标位置点的x坐标，单位是米，是以起飞点为原点的ENU坐标系，机头默认朝向正东，所以x为正即是相对于起飞点向前飞行多少米。<br/>
![](https://img-blog.csdnimg.cn/20201218084843230.png)<br/>

如下图所示，输入x坐标，并敲回车后，终端会显示如下，让你接着输入y的坐标，单位米，y值若为正则是无人机向左边飞。<br/>
![](https://img-blog.csdnimg.cn/20201218085104287.png)<br/>

然后我们输入y的坐标，因为我们是选择2D坐标点，所以只需要输入x y即可，高度z会保持不变。<br/>
![](https://img-blog.csdnimg.cn/20201218085241219.png)<br/>
然后我们输入y的坐标并敲回车后，无人机会开始向着目标点移动，如果中途有障碍物会开始避障。rviz也会实时显示出无人机的规划路径和实际路径。

注意起飞点和目标点都尽可能远离障碍物，不要在白点区域内，否则可能出现起飞并输入目标点后无人机一直保持悬停，并没有朝向目标点移动。

偶尔会出现没有雷达数据，rviz没有激光雷达图像，拔掉激光雷达的USB再插上。




