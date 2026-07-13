ROS 1コード
talkerというROS 1パッケージがあり、それがtalkerというノードでroscppが使用されているとします。
まず、新しいワークスペースを作成し、ROS1パッケージをクローンしましょう。
```bash
mkdir -p ~/ros2_talker/src_ros1
cd ~/ros2_talker/src_ros1
git clone git@github.com:HaisoRobotDaoo/ros1_to_ros2_tutorials.git .
```

ROS 1 ワークスペースのディレクトリレイアウトは次のとおりです。
```bash
cd ~/ros2_talker
find .
.
./src_ros1
./src_ros1/talker
./src_ros1/talker/package.xml
./src_ros1/talker/CMakeLists.txt
./src_ros1/talker/talker.cpp
```

ROS 2への移行
次に、作業するためのsrcを作成しましょう。
```bash
cd ~/ros2_talker
mkdir ./src
```

ROS 1 パッケージからソースツリーをそのワークスペースにコピーし、変更できるようにします。

```bash
cp -a ~/ros2_talker/src_ros1/talker src
```

さらに、src_ansを作成し、ROS2見本パッケージをクローンしましょう。
```bash
git clone -b ans git@github.com:HaisoRobotDaoo/ros1_to_ros2_tutorials.git
```

それでは、Antigravityを開き（コードの差分が比較できるので、おすすめです）、ノード内のC++コードから変更していきましょう。

```bash
antigravity
```

talker.cppの変更
ROS 2 C++ライブラリはrclcpp、ROS 1 C++ライブラリroscppが提供するAPIとは異なるAPIを提供します。
2つのライブラリの概念は非常に似ているため、変更は比較的簡単に行えます。
具体的にはROS2はC++の構文に準拠した記法に変更されているため、その点に着目しながら移行を進めるとわかりやすいかと思います。
インクルードヘッダーの変更
```cpp
//#include "ros/ros.h"
#include "rclcpp/rclcpp.hpp"
```

rclcpp=ros client library for c++
メッセージ定義の取得（名前空間）の変更

```cpp
//#include "std_msgs/String.h"
#include "std_msgs/msg/string.hpp"
```

C++ライブラリ呼び出しの変更
ノードの名前をライブラリの初期化呼び出しに渡す代わりに、初期化を行ってからノード名をノードオブジェクトの作成に渡します。
大きな変化のポイント
① 「共有ポインタ (std::shared_ptr)」の利用
ROS 2では、ノードは auto と make_shared を使って共有ポインタとして作成するのが一般的です。
これにより、メモリ管理が自動化され、他の関数やクラスへノードを引き渡す際のリスクが激減しました。
具体的にはノードのメモリ解放はすべての同一名ノードが終了してからになります。
② NodeHandle の廃止
ROS 1では「通信の窓口」として NodeHandle を作っていましたが、ROS 2ではノード自身が通信機能（Publisher/Subscriberの作成など）をすべて持っています。 
窓口を通さず、ノードに直接「これを作って」と命令する形に変わりました。
```cpp
//  ros::init(argc, argv, "talker");
//  ros::NodeHandle n;
    rclcpp::init(argc, argv);
    auto node = rclcpp::Node::make_shared("talker");
```

auto=型推論
パブリッシャーオブジェクトとレートオブジェクトの作成は、名前空間とメソッドの名前が少し変更されている以外は、非常に似ています。
```cpp
//  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
//  ros::Rate loop_rate(10);
  auto chatter_pub = node->create_publisher<std_msgs::msg::String>("chatter",
    1000);
  rclcpp::Rate loop_rate(10);
```

メッセージ配信の処理方法をさらに制御するには、サービス品質（QoS）プロファイルを渡すことができます。
デフォルトのプロファイルはrmw_qos_profile_defaultです。
詳細については、 設計ドキュメント と概念の概要を参照してください。
送信メッセージの作成は名前空間によって異なります。
```cpp
//  std_msgs::String msg;
std_msgs::msg::String msg;
```

ros::ok()の代わりにrclcpp::ok()を呼び出します。

```cpp
//  while (ros::ok())
  while (rclcpp::ok())
```

公開ループ内では、data以前と同じようにフィールドにアクセスします。
```cpp
msg.data = ss.str();
```

コンソールメッセージの変更
主な違いは、 RCLCPP_INFO()が最初の引数として Logger オブジェクトを取ることです。

```cpp
//    ROS_INFO("%s", msg.data.c_str());
    RCLCPP_INFO(node->get_logger(), "%s\n", msg.data.c_str());
```

.の代わりに->演算子を使用するようにパブリッシュ呼び出しを変更します。
```cpp
//    chatter_pub.publish(msg);
    chatter_pub->publish(msg);
```

スピニング (つまり、通信システムが保留中の受信/送信メッセージを、作業がなくなるまで処理できるようにする) は、呼び出しがノードとタイムアウトを引数として受け取るという点で異なります。

```cpp
//    ros::spinOnce();
//    rclcpp::spin_all(node, 0s);
      rclcpp::spin_some(node);
```

レートオブジェクトを使用したスリープは変更されません。
すべてをまとめると、新しいtalker.cppは次のようになります。


```cpp
// #include <chrono>
#include <sstream>
// #include "ros/ros.h"
#include "rclcpp/rclcpp.hpp"
// #include "std_msgs/String.h"
#include "std_msgs/msg/string.hpp"

// using namespace std::chrono_literals;

int main(int argc, char **argv)
{
//  ros::init(argc, argv, "talker");
//  ros::NodeHandle n;
  rclcpp::init(argc, argv);
  auto node = rclcpp::Node::make_shared("talker");
//  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
//  ros::Rate loop_rate(10);
  auto chatter_pub = node->create_publisher<std_msgs::msg::String>("chatter", 1000);
  rclcpp::Rate loop_rate(10);
  int count = 0;
//  std_msgs::String msg;
  std_msgs::msg::String msg;
//  while (ros::ok())
  while (rclcpp::ok())
  {
    std::stringstream ss;
    ss << "hello world " << count++;
    msg.data = ss.str();
//    ROS_INFO("%s", msg.data.c_str());
    RCLCPP_INFO(node->get_logger(), "%s\n", msg.data.c_str());
//    chatter_pub.publish(msg);
    chatter_pub->publish(msg);
//    ros::spinOnce();
//    rclcpp::spin_all(node, 0s);
    rclcpp::spin_some(node);

    loop_rate.sleep();
  }
  return 0;
}
```

package.xmlの変更
ROS 2パッケージは、catkinの代わりにament_cmake_rosのCMake関数とマクロを使用します。
catkinへの依存関係を削除します。
```xml
<!-- delete this -->
<buildtool_depend>catkin</buildtool_depend>
```

ament_cmake_rosに新しい依存関係を追加します。
```xml
<buildtool_depend>ament_cmake_ros</buildtool_depend>
```

ROS 2 C++ ライブラリは、roscpp
https://index.ros.org/p/roscpp/#noeticの代わりにrclcpp
https://index.ros.org/p/rclcpp/#humbleを使用します。
roscpp依存関係を削除します。

```xml
<!-- delete this -->
<depend>roscpp</depend>
```

rclcpp依存関係を追加します。

```xml
<depend>rclcpp</depend>
```

colcon にcatkinパッケージではなくament_cmakeパッケージであることを伝える<export>セクションを追加します。
```xml
<export>
  <build_type>ament_cmake</build_type>
</export>
```

現在package.xmlは次のようになっています。

```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format2.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="2">
  <name>talker</name>
  <version>0.0.0</version>
  <description>talker</description>
  <maintainer email="gerkey@example.com">Brian Gerkey</maintainer>
  <license>Apache-2.0</license>
  <buildtool_depend>ament_cmake</buildtool_depend>
  <depend>rclcpp</depend>
  <depend>std_msgs</depend>
  <export>
    <build_type>ament_cmake</build_type>
  </export>
</package>
```

CMakeLists.txtの変更
ament_cmake関数が正しく動作するには、CMake の新しいバージョンが必要です。
```cmake
cmake_minimum_required(VERSION 3.14.4)
```

REP 2000https://reps.openrobotics.org/rep-2000/で対象のROSディストリビューションが使用しているバージョンと一致する、より新しいC++標準を使用してください。
C++17を使用している場合は、project(talker)呼び出し後に以下のスニペットでそのバージョンを設定してください。
コンパイラチェックも追加することをお勧めします。

```cmake
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()
```

find_package(catkin ...)呼び出しを依存関係ごとに個別の呼び出しに置き換えます。
```cmake
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
```

catkin_package()への呼び出しを削除します。
CMakeLists.txtの下部にament_package()への呼び出しを追加します。

```cmake
ament_package()
```

target_link_librariesの呼び出しを、rclcppおよび std_msgsが提供する最新の CMake ターゲットに対して行います。
```cmake
target_link_libraries(talker PUBLIC
  rclcpp::rclcpp
  ${std_msgs_TARGETS})
```

 include_directories()の呼び出しを削除してください。
add_executable(talker talker.cpp) の直下に target_include_directories().の呼び出しを追加してください。
rclcpp_INCLUDE_DIRS のような変数を target_include_directories()に渡さないでください。
インクルードディレクトリは、最新の CMake ターゲットではtarget_link_libraries() の呼び出しによって既に処理されています。

```cmake
target_include_directories(talker PUBLIC
   "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
   "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")
```

install() の呼び出しを変更し、talker実行ファイルがプロジェクト固有のディレクトリにインストールされるようにしてください。

```cmake
install(TARGETS talker
  DESTINATION lib/${PROJECT_NAME})
```

新しいCMakeLists.txtは次のようになります。
```cmake
cmake_minimum_required(VERSION 3.14.4)
project(talker)
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
add_executable(talker talker.cpp)
target_include_directories(talker PUBLIC
   "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
   "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")
target_link_libraries(talker PUBLIC
  rclcpp::rclcpp
  ${std_msgs_TARGETS})
install(TARGETS talker
  DESTINATION lib/${PROJECT_NAME})
ament_package()
```
