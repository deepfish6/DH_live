# WSL环境设置指南

本文档提供在WSL（Windows Subsystem for Linux）环境中设置和运行实时直播数字人项目的详细步骤。

## 1. 安装WSL

如果您尚未安装WSL，请在PowerShell中以管理员身份运行以下命令：

```powershell
wsl --install
```

这将安装默认的Ubuntu发行版。安装完成后重启计算机。

## 2. 基本环境设置

启动WSL终端（在开始菜单中搜索并打开'Ubuntu'），然后运行以下命令更新系统并安装基本依赖：

```bash
# 更新系统包
sudo apt update && sudo apt upgrade -y

# 安装基本开发工具
sudo apt install -y build-essential git wget

# 安装Python和pip
sudo apt install -y python3 python3-pip python3-dev

# 安装ffmpeg（视频处理必需）
sudo apt install -y ffmpeg
```

## 3. 安装CUDA（如果需要GPU加速）

如果您需要GPU加速，请按照NVIDIA官方指南为WSL安装CUDA：

1. 确保您的Windows主机已安装最新的NVIDIA驱动程序
2. 在WSL中安装CUDA工具包：

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.2.0/local_installers/cuda-repo-wsl-ubuntu-12-2-local_12.2.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-12-2-local_12.2.0-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-12-2-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda
```

## 4. 导航到项目目录

在WSL中，Windows的驱动器会挂载在/mnt目录下。导航到项目目录：

```bash
cd /mnt/d/DH_live1/DH_live
```

## 5. 安装Python依赖

安装项目所需的Python依赖：

```bash
pip3 install -r requirements.txt
```

## 6. 使用修改版脚本

我们已经创建了一个适用于WSL环境的修改版train.sh脚本：

```bash
# 给脚本添加执行权限
chmod +x train_wsl.sh

# 执行脚本
./train_wsl.sh
```

## 常见问题解决

### Python路径问题

如果遇到Python路径问题，可能是因为WSL无法直接使用Windows的Python可执行文件。解决方法是使用WSL自己的Python：

```bash
# 编辑train_wsl.sh文件
nano train_wsl.sh

# 将最后一行修改为使用系统Python
python3 webui_train.py
```

### CUDA相关问题

如果遇到CUDA相关问题，请确保：

1. Windows主机上已正确安装NVIDIA驱动
2. WSL中已正确安装CUDA工具包
3. 可以通过运行以下命令验证CUDA安装：

```bash
nvcc --version
```

### 文件权限问题

如果遇到文件权限问题，可以尝试：

```bash
# 为所有脚本添加执行权限
chmod +x *.sh
```

## 其他注意事项

1. WSL和Windows共享文件系统，但路径表示方式不同
2. 在WSL中，Windows的C盘路径为`/mnt/c/`，D盘路径为`/mnt/d/`，以此类推
3. 如果项目依赖于特定的Windows功能，可能需要额外配置或无法在WSL中运行