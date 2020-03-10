# 参考URLメモ

* 第2回自動運転AIチャレンジ  
https://www.jsae.or.jp/jaaic/index.html
* AIチャレンジ参加者用リポジトリ  
https://github.com/tier4/aichallenge_bringup
* markdownチートシート  
https://qiita.com/Qiita/items/c686397e4a0f4f11683d
* CUDAのインストール参考  
https://qiita.com/JeJeNeNo/items/b30597918db3781e20cf

# ubuntu16.04.6 LTSインストールメモ

* なんか日本語キーボード指定しても@使えなかったので、とりあえずパスワードは記号使わず簡潔に。

* IPアドレス固定
```
# vi /etc/network/interfaces
auto enp9s0
iface enp9s0 inet static
address 192.168.0.10
netmask 255.255.255.0
gateway 192.168.0.1
dns-nameservers 192.168.0.1
```



# autoware インストールメモ
* ROSリポジトリを登録
```
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
$ sudo apt-key adv --keyserver 'hkp://ha.pool.sks-keyservers.net:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
$ sudo apt-get update
```
* 依存パッケージのインストール
```
$ sudo apt-get update
$ sudo apt-get install -y python-catkin-pkg python-rosdep ros-kinetic-catkin gksu
$ sudo apt-get install -y python3-pip python3-colcon-common-extensions python3-setuptools python3-vcstool
$ pip3 install -U setuptools
```
## CUDAインストール
https://docs.nvidia.com/cuda/archive/9.0/cuda-installation-guide-linux/index.html

* CUDAのベースインストーラーをダウンロード  
https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=debnetwork  
または以下よりwget
```
wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
```
* パッケージーマネージャーインストール
```
sudo dpkg -i cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda
```


# LGSVLシミュレータメモ
