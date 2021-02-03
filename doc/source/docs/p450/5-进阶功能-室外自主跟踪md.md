# 进阶功能-室外自主跟踪
尽可能单一的背景环境，不要出现其他圆形。<br/>
## 硬件
P200无人机（410轴距，加上桨叶斜边最长67cm，加上桨叶正方向边长50cm）<br/>
桨叶防护罩（一定要有，将桨叶上下都包住）<br/>
4S电池<br/>
富斯i6s遥控器<br/>
GPS<br/>
单目摄像头（购买链接：[购买链接](https://item.taobao.com/item.htm?_u=g5bpko475d4&id=605447137649)）<br/>
TX2<br/>
圆框（直径一米，圆心高1.45米）<br/>
![](https://img-blog.csdnimg.cn/20210203211242937.png)<br/>
尽可能单一的背景环境，不要出现其他圆形。<br/>
## 软件
prometheus<br/>

## 摄像头标定（如果标定过可不用重新标定）
具体步骤见[常见问题处理](https://prometheus-wiki.readthedocs.io/zh_CN/latest/docs/p450/4-%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E5%A4%84%E7%90%86md.html)中的摄像头标定



## 圆形检测确认
测量圆框直径，并除以2得到半径，将半径数值写入到 camera_param.yaml 文件的ellipse_det_r参数后面，将其数值修改为实际的圆框半径，修改后保存。
camera_param.yaml文件位于/Prometheus/Modules/object_detection/config/文件夹下
![](https://img-blog.csdnimg.cn/20201214143011558.png)
上面主要是更改那个相机配置文件，要写入相机标定参数和圆的半径，当然相机之前标定过就不需要重新标定了。
打开三个终端分别输入以下命令运行
```
roslaunch prometheus_detection web_cam0.launch
```
```
roslaunch prometheus_detection ellipse_det.launch
```
```
rostopic echo /prometheus/object_detection/ellipse_det
```
查看下面窗口打印的消息中position的第三个值，也是红框处的值，也即是摄像头到圆的距离值，单位是米，可以和卷尺测量的实际长度进行对比，看无人机检测到的圆框距离和现实中的距离误差大不大，一般情况下是非常准确的，这样我们可以进行下一步，
![](https://img-blog.csdnimg.cn/20201216091044822.png)
## 坐标系确认
主要是保证摄像头在无人机上安装正确
打开两个终端，分别输入以下命令
```
roslaunch prometheus_detection web_cam0.launch
```
```
rqt_image_view
```
在弹出的图形界面选择/prometheus/camera/rgb/image_raw图像消息进行查看，确保摄像头图像显示上方为现实中的上方，也就是显示的图像的的方向和现实中摄像头所拍摄到的图像的方向一致（如下图所示），代表摄像头方位正确，这样便确定摄像头的上方是哪个方向，确定好摄像头的上方之后，将相机镜头朝向机头方向固定在无人机机头方向一面，严格正前方，不要有倾斜，而且摄像头上方严格垂直向上，不要有倾斜，这样固定好摄像头后可以确保整个坐标系及后面的处理是正确的。
![](https://img-blog.csdnimg.cn/20201215094617891.png)
## 圆框跟踪
建议在背景环境单一的地方飞行，尽量不要让摄像头视角内出现其他圆形。<br/>
![](https://img-blog.csdnimg.cn/20210203205659731.png)<br/>
先将无人机环绕周围看是摄像头否识别到其他的圆，以防出现误识别，确认只能识别到圆框之后，可以开始以下步骤。
先打开遥控器，并切到定点模式，此时不需要解锁，后面程序会自动解锁并起飞，打开遥控器是为了后面以防无人机出现飞行不符合预期及时转为手动控制。
依次打开终端启动一下命令：
```
roslaunch prometheus_experiment prometheus_px4_realsense_circlecrossing_gps.launch
```
```
rostopic echo /prometheus/object_detection/ellipse_det
```
```
rqt_image_view
```


在弹出的rqt_image_view图形界面选择/prometheus/camera/rgb/image_ecllipse_det/compressed图像消息进行显示，可以显示出圆框的实时检测效果，查看是否检测到圆框。
![](https://img-blog.csdnimg.cn/20210203204227628.png)<br/>
以上确认没有问题之后

可以在下面这个终端输入1并敲回车，无人机起飞到设定的高度1米
![](https://img-blog.csdnimg.cn/20210203210206342.png)<br/>

再输入1并敲回车，无人机开始圆框跟踪，飞到距离圆框中心点3.5米处悬停。
![](https://img-blog.csdnimg.cn/20210203204812935.png)<br/>

悬停期间，可以人手动缓慢移动圆框，会看到无人机会跟着圆框移动，始终保持在距离圆框圆心前3.5米处，实现圆框跟踪。
圆框跟踪一段时间后，可以手动降落无人机，操作是遥控器在定点模式（SWC切到中档）下，SWD切入offboard再切出offboard（SWD由原本的上档切到下档再立马切回上档），此时可以转为手动控制无人机，无人机此时为基于GPS的定点模式，手动操控无人机降落。


如果在圆框跟踪过程中，发现无人机飞行状态不符合预期，也可以转为遥控器手动控制无人机，操作同上，即遥控器在定点模式（SWC切到中档）下，SWD切入offboard再切出offboard（SWD由原本的上档切到下档再立马切回上档），此时可以转为手动控制无人机，无人机此时为基于GPS的定点模式，可以遥控器控制无人机飞行。（在出现不符合预期的情况下可以这么做，紧急转为手动控制住无人机，以免造成炸机）


再次飞注意通过地面站发送reboot重启飞控。





