# ROS 2演習 2023

Windows 10/11 WSL2（Ubuntu 22.04/Ubuntu 20.04）での演習を想定しています。
動作確認は、Windows 11 WSL2（Ubuntu 22.04）で行いました。

## ROS 2の構築

- APTリポジトリの追加・更新
curl等のインストール
```shell
sudo apt update && sudo apt install curl gnupg lsb-release
ROS GPGキーの入手（反応がない場合はネットワークを変更する：埼玉大学内ネットワークでつながらないことがたまにある）
```shell
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

APTリポジトリの追加・更新
```shell
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

```

APTリポジトリの更新
```shell
sudo apt update
```

- ROS 2のインストール (ROS 2 Version Humble)

```shell
sudo apt install ros-humble-desktop
```

- 環境設定
```shell
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

- colcon build インストール
```shell
sudo apt install python3-colcon-common-extensions
```

- ROS2 コマンド自動補完のインストール
```shell
sudo apt install python3-argcomplete
```

## 環境設定
<!-- 
- colconの設定
```shell
echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
echo "export _colcon_cd_root=~/ros2_install" >> ~/.bashrc
```
-->
- 環境の確認
```shell
 printenv | grep -i ROS_
```

出力例
```
ROS_VERSION=2  
ROS_PYTHON_VERSION=3 
ROS_LOCALHOST_ONLY=0 
ROS_DISTRO=humble
```

- domain_idの設定

> export ROS_DOMAIN_ID=<your_domain_id>

your_domain_idは0～65532で選択

your_domain_idが１の場合の例
```
echo "export ROS_DOMAIN_ID=1">> ~/.bashrc
```

変更した設定を反映
```shell
source ~/.bashrc
```

インストール確認：先にVcxSrvの起動しておくこと
```shell
ros2 run turtlesim turtlesim_node
```
