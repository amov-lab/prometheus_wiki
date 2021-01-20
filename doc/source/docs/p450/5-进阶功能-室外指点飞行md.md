# 进阶功能-室外指点飞行


## 软件和硬件环境
### 硬件
P200无人机
TX2
Holybro pixhawk4
GPS
### 软件
1.10.0版本PX4固件
ubuntu16.04
nomachine（下载：[https://www.nomachine.com/](https://www.nomachine.com/)，使用方法：[nomachine使用方法](https://www.ncnynl.com/archives/202007/3809.html)）
## 飞控参数修改
EKF2_AID_MASK 	选择GPS
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118095225519.png)

EKF2_HGT_MODE 	选择气压计作为作为高度估计的主要来源。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118095243549.png)


## 软件环境搭建
####   安装prometheus
  参考  [promethues安装与编译](https://github.com/amov-lab/Prometheus/wiki/%E5%AE%89%E8%A3%85%E5%8F%8A%E7%BC%96%E8%AF%91)

## 硬件连接
准备下面这一条线
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112711014747.png)TX2一端这么插
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201127110050829.png)
数据线的另一端接飞控telem2口，波特率为921600

## 命令运行
无人机上电后等待GPS由蓝灯慢闪变为绿灯慢闪，再执行以下操作。
使用nomachine远程登录TX2
打开一个新终端，运行以下命令
```
roslaunch prometheus_experiment prometheus_px4_realsense.launch
```
## 实际飞行
在弹出的以下终端先输入0敲回车，选择命令输入控制

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021011809100474.png)
再输入4，选择move指令（这里以move为例）


![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118092754801.png)

然后输入0并敲回车，表示在X Y Z方向上都进行位置控制，发送X Y Z上的期望位置点。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118092842629.png)

然后输入1并敲回车，表示选择BODY坐标系（强烈建议室外选择BODY坐标系）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118092943313.png)

然后接着输入期望的BODY坐标系下的无人机的X轴坐标并敲回车
这里对坐标系做下说明，x为正代表往机头方向飞，y为正代表往无人机左边飞，z为正代表无人机往上飞。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118090756850.png)
然后接着输入y的坐标并敲回车
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118093210828.png)
接着输入Z的坐标并敲回车
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118093318669.png)
接着输入期望的偏航并敲回车（一般为0）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118093434421.png)
敲回车后终端会显示如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118093602113.png)
遥控器定点模式（SWC切到中档）下解锁并切到offboard模式（SWC切到下档），无人机会自动起飞到期望位置点，我上面演示设置的就是飞到无人机正上方一米处，偏航不变。


指点飞行建议在BODY坐标系下。
