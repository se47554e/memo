# ubuntu18.04.4 LTSにAutowareのDockerコンテナを動かす

## dockerをインストール
```
sudo apt-get remove docker docker-engine docker.io
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88

### 表示される内容
### pub 4096R/0EBFCD88 2017-02-22
### Key fingerprint = 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88
### uid Docker Release (CE deb)
### sub 4096R/F273FCD8 2017-02-22

# リポジトリの追加
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
# インストール
sudo apt-get install docker-ce
# 確認
sudo docker run hello-world

# dockerのバージョン確認
sudo docker -v
```

## nvidia-docker2インストール
autowareのdockerコンテナがruntimeオプションを使うため、nvidia-docker2を入れる必要あり  
dockerのバージョン19.03以降はdockerでGPUを使うとき、nvidia-container-toolkitが推奨  
このあたり参考?
https://github.com/NVIDIA/nvidia-docker/tree/master#quickstart
```
# リポジトリの追加
# 何やったか忘れたので後日再検証

# インストール
sudo apt-get isntall nvidia-docker2

# 確認
docker run --runtime nvidia nvidia/cuda:10.0-base nvidia-smi
```

## nvidia-container-toolkitのインストール
dokcerのバージョンが19.03以降の場合、dockerでGPUを使う際はこれをつかう
autowareを使うにあたり、必要か不明。とりあえずインストール。
```
# リポジトリの追加？   
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
$ curl -s -L https://nvidia.github.io/nvidia-docker/$(. /etc/os-release;echo $ID$VERSION_ID)/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
$ sudo apt update

# インストール
$ sudo apt -y install nvidia-container-toolkit
$ sudo shutdown -r now

# 確認
nvidia-container-cli info

# 確認
docker run --gpus all nvidia/cuda:10.0-base nvidia-smi
```

## 一般ユーザがdockerコマンドを使えるようにする必要あり
```
sudo usermod -aG docker $USER
reboot
```

## autoware docker containerを動かす
```
git clone https://gitlab.com/autowarefoundation/autoware.ai/docker.git
cd docker/generic
./run.sh -t 1.13.0
```