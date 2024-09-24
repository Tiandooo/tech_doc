## 基础信息

GPU: A100 40G

OS: ubuntu 22.04

安装cuda版本:11.8

## 1. 安装nvidia驱动



```bash
sudo add-apt-repository ppa:graphics-drivers/ppa

sudo apt update

ubuntu-drivers devices
# 安装需要的版本
sudo apt install nvidia-driver-535-server
```



重启后即可nvidia-smi看见



## 2. 安装cuda

```bash
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run

# 去掉勾选的driver，只安装cuda
sudo sh cuda_11.8.0_520.61.05_linux.run

```

配置环境变量

##### 1. 打开profile 文件

```bash
vim /etc/profile
```

##### 2. 添加环境变量

```bash
export PATH="$PATH:/usr/local/cuda-11.8/bin"
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda-11.8/lib64/"
```



## 3. 安装cudnn

从cudnn官网下载tar包，上传后

```bash
tar -xvf cudnn-linux-x86_64-8.9.7.29_cuda11-archive.tar

cd cudnn-linux-x86_64-8.9.7.29_cuda11-archive/

sudo cp include/cudnn*    /usr/local/cuda-11.8/include

sudo cp lib/libcudnn*    /usr/local/cuda-11.8/lib64

sudo chmod a+r /usr/local/cuda-11.8/include/cudnn*   /usr/local/cuda-11.8/lib64/libcudnn*
```



验证：

```bash
cat /usr/local/cuda-11.8/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```



## 4. 安装anaconda



```bash
curl -O https://repo.anaconda.com/archive/Anaconda3-2024.06-1-Linux-x86_64.sh

chmod +x Anaconda3-2024.06-1-Linux-x86_64.sh

./Anaconda3-2024.06-1-Linux-x86_64.sh
```



配置环境变量

##### 1. 打开profile 文件

```bash
vim /etc/profile
```

##### 2. 添加环境变量

```bash
export PATH=/root/anaconda3/bin:$PATH
```

