正確なコマンドは、ROS 2 をインストールした場所によって異なります。問題が発生した場合は、ファイルパスがインストールにつながっていることを確認してください。
コマンドのより詳細な説明や利用シーンでお困りの場合は下記リンクと関連ワークスペースをご確認ください。

セットアップスクリプトのソース
```bash
source /opt/ros/humble/setup.bash
```

ROS_DOMAIN_ID環境変数の設定
```bash
export ROS_DOMAIN_ID=<your_domain_id>
```


ROS_LOCALHOST_ONLY環境変数の設定
```bash
export ROS_LOCALHOST_ONLY=1
```


ROS環境変数の確認
```bash
printenv | grep -i ROS
```


ROS 2 ディストリビューション用のパッケージをインストール
```bash
sudo apt install ros-<distro>-<package_name>
```


ROS 2 ディストリビューション用のパッケージをインストール
```bash
sudo apt install ros-<distro>-<package_name>
```


パッケージがインストールされているかどうか、そのパッケージの中に「実行できるプログラム（ノード）が何があるか」を確認
```bash
ros2 pkg executables <package_name>
```


ROS2の実行
```bash
ros2 run <package_name> <executable_name>
```


ノード名のリマッピング
```bash
ros2 run <package_name> <executable_name> --ros-args --remap __node:=<new_node_name>
```


トピック名のリマッピング
```bash
ros2 run <package_name> <executable_name> --ros-args --remap <topic_name>:=<new_topic_name>
```

ノードリスト
```bash
ros2 node list
```

ノード情報
```bash
ros2 node info 
```

トピックリスト
```bash
ros2 topic list
```


トピックの種類を括弧内に追加表示したい場合、--show-types(または-t) フラグを設定
```bash
ros2 topic list -t
```

トピックエコー
```bash
ros2 topic echo <topic_name>
```

トピック情報
```bash
ros2 topic info <topic_name>
```


トピックの種類、ノード名と名前空間、QoSを取得するには、--verbose(または-v) フラグを使用
```bash
ros2 topic info <topic_name> -v
```


トピックパブ
```bash
ros2 topic pub <topic_name> <msg_type> '<{arg1: {value1: , value2: ,}, arg2: {value1: , value2: ,}, }>'
```

トピックにデータを一度だけ（継続的にではなく）パブリッシュしたい場合
```bash
ros2 topic pub --once -w 2 <topic_name> <msg_type> '<{arg1: {value1: , value2: ,}, arg2: {value1: , value2: ,}, }>'
```

トピック検索
```bash
ros2 topic find <msg_type>
```


トピックHz
```bash
ros2 topic hz <topic_name>
```


トピックbw
```bash
ros2 topic bw <topic_name>
```

サービスリスト
```bash
ros2 service list
```


サービスの種類を括弧内に追加表示したい場合、--show-types(または-t) フラグを設定
```bash
ros2 service list -t
```


サービスタイプ
```bash
ros2 service type <service_name>
```


サービスコール
```bash
ros2 service call <service_name> <service_type> <"{arg1: value, arg2: 'string', }">
```


サービス検索
```bash
ros2 service find <msg_type>
```


アクションリスト
```bash
ros2 action list
```


インターフェース表示

```bash
ros2 interface show <msg_type>
```


--symlink-installとは
```bash
colcon build --symlink-install
```

通常、ビルドを行うとファイルが install ディレクトリにコピーされます。しかし、このオプションを使うと、**「コピー」ではなく「シンボリックリンク（ショートカット）」**が作成されます。

- メリット: Pythonスクリプトやパラメータファイル（YAML）、起動ファイル（Launchファイル）などを変更した際、再ビルド（colcon build）をしなくても変更が即座に反映されます。
- 注意点: C++のコードを変更した場合は、リンク先が書き換わるわけではない（コンパイルが必要な）ので、通常通り再ビルドが必要です。
