# ubuntu 16.04.6 LTSでautowareをインストールするメモ

### ubuntu16.04.6 LTSインストールメモ

* パスワードはすべて小文字、記号なしでインストールする。インストール終わってから変える方向で。

* IPアドレス固定(必要があれば)
```
# vi /etc/network/interfaces
auto enp9s0
iface enp9s0 inet static
address 192.168.0.10
netmask 255.255.255.0
gateway 192.168.0.1
dns-nameservers 192.168.0.1
```

* ホームディレクトリ配下の日本語ファイルを英語化しておく。好みの問題。再起動時、毎回日本語に戻すか聞かれるかも。
```
LANG=C xdg-user-dirs-gtk-update
```



### CUDAインストール
* CUDAのインストール参考  
https://qiita.com/JeJeNeNo/items/b30597918db3781e20cf

* nvidiaサイト  
https://docs.nvidia.com/cuda/archive/9.0/cuda-installation-guide-linux/index.html

* CUDAのベースインストーラーをサイトからダウンロード  
https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=debnetwork  

またはwget
```
wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
```
* パッケージーマネージャーインストール  
```
sudo dpkg -i cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
sudo apt-get update
# sudo apt-get install cuda
sudo apt-get install cuda-9.0
```

```
echo -e "\n## CUDA and cuDNN paths"  >> ~/.bashrc
echo 'export PATH=/usr/local/cuda-9.0/bin:${PATH}' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64:${LD_LIBRARY_PATH}' >> ~/.bashrc
source ~/.bashrc

echo $PATH             # 出力に"/usr/local/cuda-9.0/bin"が含まれているか？
echo $LD_LIBRARY_PATH  # 出力に"/usr/local/cuda-9.0/lib64"が含まれているか？
which nvcc             # 出力が"/usr/local/cuda-9.0/bin/nvcc"になっているか？
nvidia-smi             # nvidiaのGPUの情報が表示されているか？

```
* CPU情報が表示されない場合は**1回再起動**したら読み込むかも
* 表示されない場合はNVIDIAドライバの更新？ 参考サイト 
http://www.nvidia.com/Download/index.aspx?lang=en-us

* 動作確認
```
cuda-install-samples-9.0.sh ~ # ホームディレクトリにサンプルコードをコピー。
cd ~/NVIDIA_CUDA-9.0_Samples/
make
cd 2_Graphics/volumeRender    # サンプルの実行ファイルがあるディレクトリに移動。
./volumeRender
```

## autoware インストール
* ROSリポジトリを登録
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver 'hkp://ha.pool.sks-keyservers.net:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
sudo apt-get update
```
* 依存パッケージのインストール
```
sudo apt-get update
sudo apt-get install -y python-catkin-pkg python-rosdep ros-kinetic-catkin gksu
sudo apt-get install -y python3-pip python3-colcon-common-extensions python3-setuptools python3-vcstool
pip3 install -U setuptools
```

* BUILD
```
# ワークスペースを作成する
mkdir -p autoware.ai/src 
cd autoware.ai 

# Autoware.AIのワークスペース構成をダウンロード
wget -O autoware.ai.repos "https://gitlab.com/autowarefoundation/autoware.ai/autoware/raw/1.12.0/autoware.ai.repos?inline=false" 

# Autoware.AIをワークスペースにダウンロード
vcs import src < autoware.ai.repos 

# rosdepを使用して依存関係をインストール
rosdep update
rosdep install -y --from-paths src --ignore-src --rosdistro kinetic

# ワークスペースをコンパイル
AUTOWARE_COMPILE_WITH_CUDA=1 colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release 
```

## LGSVLシミュレータメモ
* ほげ