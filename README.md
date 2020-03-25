# Docker-Mnist

# DockerでKerasを使ったディープラーニングの環境を構築する
今回はLinuxで動作させることを想定しています。  
Windowsはまた今度考えます。

## 環境
- Ubuntu 18.04.3 LTS
- GPU (Nvidia製のもの)

## Dockerをインストールする
以下のコマンドを実行
```
sudo apt -y install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```  
  
Docker のリポジトリを APT に登録する。  
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
```  
  
Dockerのコンポーネントをインストールする。
```
sudo apt -y install docker-ce docker-ce-cli containerd.io
```
  
`sudo docker version`で以下のような結果が返ってくればOK。  
```
Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b7f0
 Built:             Wed Mar 11 01:25:46 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b7f0
  Built:            Wed Mar 11 01:24:19 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```  

## NVIDIA Graphics Driver をインストールする
ドライバのリポジトリを APT に登録する。  
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
```  
  
推奨されているドライバをインストールする。
```
sudo apt -y install ubuntu-drivers-common
sudo ubuntu-drivers autoinstall
```  

## NVIDIA Container Toolkit をインストールする
次に Docker ホストに NVIDIA Container Toolkit をインストールする。  
この中に Docker で NVIDIA の GPU を使うのに必要なランタイムなどが含まれている。  
リポジトリを APT に登録する。
```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$(. /etc/os-release;echo $ID$VERSION_ID)/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt update
```  
  
ツールキットをインストールする。
```
sudo apt -y install nvidia-container-toolkit
```  
  
一旦再起動する。
```
sudo reboot
```  
  
最後に`nvidia-container-cli info`コマンドでGPUが認識できていれば成功。  
```
NVRM version:   440.64.00
CUDA version:   10.2

Device Index:   0
Device Minor:   0
Model:          GeForce GTX 1070 Ti
Brand:          GeForce
GPU UUID:       GPU-80c3d33f-307f-14cd-cc53-9ad31cf49d73
Bus Location:   00000000:01:00.0
Architecture:   6.1
```

# エラーが出てしまったら。。。
この辺を参照。
- https://qiita.com/aokomoriuta/items/d59b54c14ea473f2a1c3