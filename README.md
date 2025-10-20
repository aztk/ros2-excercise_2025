# ROS 2演習 2025（Ubuntu 22.04 標準・Ubuntu 24.04 対応）

Windows 10/11 WSL2（Ubuntu 22.04 または 24.04）での演習を想定しています。  
標準環境は **Ubuntu 22.04（ROS 2 Humble）** です。  
Ubuntu 24.04 でもほぼ同じ手順で動作しますが、一部コマンドやパッケージ名が異なります。  
（24.04ではROS 2 Jazzyを使用）

---

## 🔧 ROS 2の構築手順

### 1. APTリポジトリの追加・更新

#### curl等のインストール（共通）
```bash
sudo apt update && sudo apt install curl gnupg lsb-release
````

#### ROS GPGキーの入手（共通）

> 反応がない場合はネットワークを変更する。埼玉大学内ネットワークでは接続できないことがある。

```bash
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

#### APTリポジトリの追加（共通）

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

#### APTリポジトリの更新（共通）

```bash
sudo apt update
```

---

### 2. ROS 2のインストール

#### ✅ Ubuntu 22.04（Humbleの場合）

```bash
sudo apt install ros-humble-desktop
```

#### 💡 Ubuntu 24.04（Jazzyの場合）

```bash
sudo apt install ros-jazzy-desktop
```

> ⚠️ **注意**
>
> * Ubuntu 24.04 (Noble) では `ros-humble-desktop` はサポート外です。
> * 必ず `ros-jazzy-desktop` を使用してください。
> * Jazzy は Python 3.12 ベースで構築されており、Humble（Python 3.10）とは互換性がありません。

---

### 3. 環境設定

#### ✅ Ubuntu 22.04（Humbleの場合）

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

#### 💡 Ubuntu 24.04（Jazzyの場合）

```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

> 💡 `ros2 --version` コマンドは初回設定前には使えません。
> そのため、自動検出式 (`$(ros2 --version ...)`) は使用せず、手動で設定します。

---

### 4. colcon build のインストール（共通）

```bash
sudo apt install python3-colcon-common-extensions
```

---

### 5. ROS 2 コマンド自動補完（共通）

```bash
sudo apt install python3-argcomplete
```

---

## 🧭 環境設定確認

```bash
printenv | grep -i ROS_
```

出力例：

**Ubuntu 22.04 (Humble)**

```
ROS_VERSION=2  
ROS_PYTHON_VERSION=3 
ROS_LOCALHOST_ONLY=0 
ROS_DISTRO=humble
```

**Ubuntu 24.04 (Jazzy)**

```
ROS_VERSION=2  
ROS_PYTHON_VERSION=3 
ROS_LOCALHOST_ONLY=0 
ROS_DISTRO=jazzy
```

---

### 6. Domain ID設定（共通）

```bash
echo "export ROS_DOMAIN_ID=1" >> ~/.bashrc
source ~/.bashrc
```

---

### 7. インストール確認（共通）

> **GUIを利用する場合は先にVcxSrvまたはWSLgを起動しておくこと。**

```bash
ros2 run turtlesim turtlesim_node
```

---

## ✅ まとめ（22.04標準／24.04比較表）

| 項目         | Ubuntu 22.04 (標準)                     | Ubuntu 24.04 (追加情報)                  |
| ---------- | ------------------------------------- | ------------------------------------ |
| コード名       | jammy                                 | noble                                |
| ROS 2バージョン | Humble Hawksbill                      | Jazzy Jalisco                        |
| Python     | 3.10                                  | 3.12                                 |
| インストールコマンド | `sudo apt install ros-humble-desktop` | `sudo apt install ros-jazzy-desktop` |
| 環境設定       | `/opt/ros/humble/setup.bash`          | `/opt/ros/jazzy/setup.bash`          |
| GUI環境      | VcXsrv / WSLg                         | WSLg推奨                               |
| 動作確認       | ✅                                     | ✅（一部注意事項あり）                          |

---

## 💡 トラブルシューティング

| 問題                           | 原因                      | 対処                                             |
| ---------------------------- | ----------------------- | ---------------------------------------------- |
| `ros2: command not found`    | `.bashrc` に正しい設定がない     | `/opt/ros/humble` または `/opt/ros/jazzy` を明示的に指定 |
| `ros-humble-desktop` が見つからない | Ubuntu 24.04ではHumble非対応 | `ros-jazzy-desktop` に変更                        |
| `turtlesim` 実行時に画面が出ない       | DISPLAY設定が未設定           | `.bashrc` に DISPLAY設定を追加                       |
| `rosdep update` でエラー         | 初期化未実施                  | `sudo rosdep init && rosdep update`            |

---

#### 🐢 ROS 2 Humble Hawksbill（Ubuntu 22.04 対応）
- **公式ドキュメント（英語）**  
  [ROS 2 Humble Documentation](https://docs.ros.org/en/humble/)  
  - Ubuntu 22.04 (Jammy Jellyfish) に対応。  
  - [インストールガイド（.debパッケージ）](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)  
  - Python 3.10 ベース。ROS 2 Humble は LTS (長期サポート) 版。  

- **関連情報**  
  - [Installing ROS 2 Humble on Ubuntu — Foxglove Blog](https://foxglove.dev/blog/installing-ros2-humble-on-ubuntu)  
  - [ROS 2 Humble Hawksbill Release Notes](https://docs.ros.org/en/foxy/Releases/Release-Humble-Hawksbill.html)  
  - [REP-2000 対応表（Ubuntu 22.04 は Tier 1 対応）](https://www.ros.org/reps/rep-2000.html)

---

#### 🦋 ROS 2 Jazzy Jalisco（Ubuntu 24.04 対応）
- **公式ドキュメント（英語）**  
  [ROS 2 Jazzy Documentation](https://docs.ros.org/en/jazzy/)  
  - Ubuntu 24.04 (Noble Numbat) に対応するROS 2の正式リリース。  
  - [公式インストールガイド（.debパッケージ）](https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html)  
  - Python 3.12ベースで構築されており、Humbleとは非互換。  
  - Ubuntu 24.04 + WSL2 (WSLg) でのGUI動作が確認済み。  

- **関連情報・フォーラム**  
  - [Ubuntu 24.04 で ROS 2 Jazzy を動かす方法 (海外ユーザー報告)](https://www.reddit.com/r/ROS/comments/1crwzw9/hi_everyone_can_i_install_ros2_on_ubuntu_2404/)  
  - [REP-2000 （ROS 2 ディストリビューションとOS対応表）](https://www.ros.org/reps/rep-2000.html)

---

