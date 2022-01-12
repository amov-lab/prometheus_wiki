# 常见问题处理
你到论坛专区进行提问！！！！！
## 遇到问题如何处理
## 常见问题
###  摄像头标定
下面是标定板样张，注意打印出来粘在硬纸板上，一定要保证整个图片在一个平面上，否则会影响标定效果。标定效果好坏会直接影响到后面摄像头对二维码位置的估计进而影响到二维码降落，所以重视标定过程。
棋盘格标定板下载地址：[Chessboard](http://jario.ren/images/2005/qipangebiaoding.jpg)
![](https://img-blog.csdnimg.cn/20201203141922533.jpg)首先启动相机节点，如下命令启动相机ID=0<br/>
```
roslaunch prometheus_detection web_cam0.launch  
```
然后利用ros自带的标定程序对相机进行标定<br/>
```
rosrun camera_calibration cameracalibrator.py --size 8x6 --square 0.0245 image:=/prometheus/camera/rgb/image_raw
```

其中：size为标点板尺寸（横纵向标点的个数），square为每个方格宽度(即两个标点之间的距离)，注意单位是米，image:=相机话题<br/>
如果是用我们给的标定图片并且用A4纸打印出来，则size后的值为9*6 ，square后面的值以自己打印出来的两个标点的实际量测距离为准，单位是米，一般大小在0.0200左右。

标定程序启动后，会出现如下界面
在没有标定成功之前，右边的按键都为灰色，不能点击。为了提高标定的准确性，应该让标定板出现在摄像头视野范围内的各个区域，界面右上角会提示标定进度。标定过程标定板的移动可以参考此视频：[Calibrating a Monocular Camera with ROS](https://www.bilibili.com/video/BV1o7411C73L?from=search&seid=4341277568306257299)<br/>
X:标定板在摄像头视野中的左右移动<br/>
Y:标定板在摄像头视野中的上下移动<br/>
Size:标定板在摄像头视野中的前后移动<br/>
Skew:标定板在摄像头视野中的倾斜转动<br/>
不断在视野中移动标定板，直到“CALIBRATE”按钮变色，表示标定程序的参数采集完成。为了提高标定的精度，建议在X,Y,Size,Skew四个进度条都变为绿色，数据采集足够后，再按下“CALIBRATE”按钮。此时可以在终端看见一共采集了多少个有效点，建议采集到80个点左右，以保证标定的精度。
点击“CALIBRATE”按钮，标定程序开始自动计算摄像头的标定参数，这个过程需要等待一段时间，界面可能会变成灰色无响应状态，注意千万不要关闭。
![](https://img-blog.csdnimg.cn/202012072110329.png)

等待几分钟后，计算出结果会自动在终端打印出来显示，这时dispaly方框又会由黑白恢复为彩色。如下图所示
![](https://img-blog.csdnimg.cn/20201207083342502.png)
将得到的参数写入如下文件(有关目标尺度的预定义也在这个文件中)： amovlab_ws/src/p450_experiment/config/prometheus_detection_config/camera_param.yaml
具体数字对应可参考下图
![](https://img-blog.csdnimg.cn/20201203224211428.png)


## 提问问题模板
