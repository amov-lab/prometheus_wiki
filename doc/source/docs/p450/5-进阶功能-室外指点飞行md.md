# 进阶功能-室外指点飞行


## 软件和硬件环境
### 硬件
P200无人机<br/>
TX2<br/>
Holybro pixhawk4<br/>
GPS<br/>
### 软件
1.10.0版本PX4固件<br/>
ubuntu16.04<br/>
nomachine（下载：[https://www.nomachine.com/](https://www.nomachine.com/)，使用方法：[nomachine使用方法](https://www.ncnynl.com/archives/202007/3809.html)）
## 飞控参数修改
EKF2_AID_MASK 	选择GPS<br/>
![](https://img-blog.csdnimg.cn/20210118095225519.png)<br/>

EKF2_HGT_MODE 	选择气压计作为作为高度估计的主要来源。<br/>
![](https://img-blog.csdnimg.cn/20210118095243549.png)<br/>


## 软件环境搭建
####   安装prometheus
  参考  [promethues安装与编译](https://github.com/amov-lab/Prometheus/wiki/%E5%AE%89%E8%A3%85%E5%8F%8A%E7%BC%96%E8%AF%91)

## 硬件连接

准备下面这一条线<br/>
![](https://img-blog.csdnimg.cn/2020112711014747.png)<br/>

TX2一端这么插<br/>
![](https://img-blog.csdnimg.cn/20201127110050829.png)<br/>

数据线的另一端接飞控telem2口，波特率为921600

## 命令运行
无人机上电后等待GPS由蓝灯慢闪变为绿灯慢闪，再执行以下操作。
使用nomachine远程登录TX2
打开一个新终端，运行以下命令
```
roslaunch prometheus_experiment prometheus_px4_realsense.launch
```
## 实际飞行

在弹出的以下终端先输入0并敲回车，选择命令输入控制<br/>
![](https://img-blog.csdnimg.cn/2021011809100474.png)<br/>

再输入4，选择move指令（这里以move为例）<br/>
![](https://img-blog.csdnimg.cn/20210118092754801.png)<br/>

然后输入0并敲回车，表示在X Y Z方向上都进行位置控制，发送X Y Z上的期望位置坐标。<br/>
![](https://img-blog.csdnimg.cn/20210118092842629.png)<br/>

然后输入1并敲回车，表示选择BODY坐标系（强烈建议室外选择BODY坐标系）<br/>
![](https://img-blog.csdnimg.cn/20210118092943313.png)<br/>

然后接着输入期望的BODY坐标系下的无人机的X轴坐标并敲回车。<br/>
这里对坐标系做下说明，x为正代表往机头方向飞，y为正代表往无人机左边飞，z为正代表无人机往上飞。<br/>
![](https://img-blog.csdnimg.cn/20210118090756850.png)<br/>

然后接着输入Y的坐标并敲回车<br/>
![](https://img-blog.csdnimg.cn/20210118093210828.png)<br/>

接着输入Z的坐标并敲回车<br/>
![](https://img-blog.csdnimg.cn/20210118093318669.png)<br/>

接着输入期望的偏航并敲回车（一般为0）<br/>
![](https://img-blog.csdnimg.cn/20210118093434421.png)<br/>

敲回车后终端会显示如下<br/>
![](https://img-blog.csdnimg.cn/20210118093602113.png)<br/>

然后遥控器切到定点模式（SWC切到中档）下解锁，再切到offboard模式（SWD切到下档，SWC不需要动），无人机会自动起飞到期望位置点，我上面演示设置的就是飞到无人机正上方一米处，偏航不变。

室外GPS指点飞行建议选择BODY坐标系发送期望位置坐标。

如果无人机出现失控意外不符合预期或者想转为手动控制只需遥控器切出offboard模式（即SWD切到上档，SWC不需要动，保持中档），无人机会退出offboard模式并自动转为position模式也就是定点模式，此时可以通过遥控器手动操控无人机。
