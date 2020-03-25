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
$ sudo apt -y install \
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