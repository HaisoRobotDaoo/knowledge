catkin のワークスペースを作成する
[あなたがボットでないことを確認しています！](https://wiki.ros.org/catkin/Tutorials/create_a_workspace)
ROS パッケージの作成
[あなたがボットでないことを確認しています！](https://wiki.ros.org/catkin/Tutorials/CreatingPackage)

ワークスペースの作成からビルドまで
ROSはcatkin-toolsというツールをビルドします
```bash
sudo apt install python3-catkin-tools

echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
```

ワークスペースの作成および移動
catkin_wsという名前はあくまで一例。都度、状況に併せて名前を変える。

```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
```


ワークスペースの初期設定
```bash
catkin init
```

パッケージの新規作成（必要であれば）

```bash
catkin create pkg パッケージ名
```



ワークスペースのビルド
```bash
catkin build
```

ワークスペースの反映

```bash
source devel/setup.bash
```



簡単なトピック通信の作成
以下のチュートリアルを参照
[あなたがボットでないことを確認しています！](https://wiki.ros.org/ja/ROS/Tutorials/WritingPublisherSubscriber%28python%29)
簡単なサービス通信の作成
[あなたがボットでないことを確認しています！](https://wiki.ros.org/ja/ROS/Tutorials/WritingServiceClient%28python%29)

ROSの基本的なコマンド
roscore　全ての中心となるサーバーを立てる。ネットワークに１つ
rosrun
rosluanch
rostopic list

画像処理を行うためのROS-OpenCVを繋ぐパッケージ
あなたがボットでないことを確認しています！
