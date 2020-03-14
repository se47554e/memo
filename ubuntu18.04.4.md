# ubuntu 18.04.4 LTSでautowareをインストールするメモ

### ubuntu16.04.6 LTSインストールメモ
* 適宜パッケージの追加
```
sudo apt-get update
sudo apt-get install openssh-server net-tools vim git
```
* 静的IPアドレス設定
> GUIの優先設定から手動で変更、再適用させる。

### CUDA 10.0インストール
* 参考サイト  
https://qiita.com/yukoba/items/4733e8602fa4acabcc35
* nvidiaサイト  
https://docs.nvidia.com/cuda/archive/10.0/cuda-installation-guide-linux/index.html

* CUDA Toolkit 10.0 をインストール
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo add-apt-repository "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/ /"
sudo apt-get update
sudo apt-get -y install cuda-10.0
```
* パスを通す
```
echo -e "\n## CUDA and cuDNN paths"  >> ~/.bashrc
echo 'export PATH=/usr/local/cuda-10.0/bin:${PATH}' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:${LD_LIBRARY_PATH}' >> ~/.bashrc
source ~/.bashrc
```
* PC再起動
```
echo $PATH             # 出力に"/usr/local/cuda-9.0/bin"が含まれているか？
echo $LD_LIBRARY_PATH  # 出力に"/usr/local/cuda-9.0/lib64"が含まれているか？
which nvcc             # 出力が"/usr/local/cuda-9.0/bin/nvcc"になっているか？
nvidia-smi             # nvidiaのGPUの情報が表示されているか？
```
* 動作確認
```
cuda-install-samples-10.0.sh ~ # ホームディレクトリにサンプルコードをコピー。
cd ~/NVIDIA_CUDA-10.0_Samples/
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
sudo apt update
sudo apt install -y python-catkin-pkg python-rosdep ros-melodic-catkin
sudo apt install -y python3-pip python3-colcon-common-extensions python3-setuptools python3-vcstool
pip3 install -U setuptools
```

* BUILD
```
# ワークスペースを作成する
mkdir -p autoware.ai/src 
cd autoware.ai 

# Autoware.AIのワークスペース構成をダウンロード
wget -O autoware.ai.repos "https://gitlab.com/autowarefoundation/autoware.ai/autoware/raw/1.13.0/autoware.ai.repos?inline=false" 

# Autoware.AIをワークスペースにダウンロード
vcs import src < autoware.ai.repos 

# 不要なパッケージは削除したほうがいい？
sudo apt-get clean
sudo apt-get autoremove

# rosdepを使用して依存関係をインストール(rosdep init は必要？)
sudo rosdep init
rosdep update
rosdep install -y --from-paths src --ignore-src --rosdistro melodic

# ワークスペースをコンパイル
AUTOWARE_COMPILE_WITH_CUDA=1 colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release 
```
## LGSVLシミュレータメモ