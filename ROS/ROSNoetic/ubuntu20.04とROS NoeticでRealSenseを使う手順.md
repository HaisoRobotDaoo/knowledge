Intel® RealSense™ デバイス用 ROS ラッパー
Intel® RealSense™ROSのインストール〜ワークスペースで実行
[GitHub - IntelRealSense/realsense-ros at ros1-legacy](https://github.com/IntelRealSense/realsense-ros/tree/ros1-legacy?tab=readme-ov-file)
Intel® RealSense™ SDK 2.0
SDKのインストール〜起動
[librealsense/doc/distribution_linux.md at master · IntelRealSense/librealsense](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md#installing-the-packages)
ターミナル①
```bash
roscore
```
ターミナル②
```bash
roslaunch realsense2_camera rs_camera.launch
```
ターミナル③
```bash
rosrun pkg名 実行ファイル.py
```
※
このとき実行ファイルのサブスクライバで
```python
rgb_sub = rospy.Subscriber("/camera/color/image_raw", Image, self._sub_callback)
```
みたいにimage_topicの部分にパスを指定する必要がある
