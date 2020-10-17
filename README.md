# Linux_reference
关于linux的相关操作参考

# 一.在Ubuntu上安装CUDA【单系统方法】

## - 配置环境  
Ubuntu 16.04 <-> kernel vision 4.4.0.31  
NVIDIA GPU dirve 384.130  
CUDA Toolkit 8.0(GA2)  
cuDNN 8.0-linux-x64-v7.1  
  
##  -安装过程  
#### 1.确定系统 kernel vision 和 system vision；  
![image](https://github.com/HouYueJie/Linux_reference/blob/master/CUDA_IMG/1.png)    

#### 2.查看gcc 版本情况:  
    sudo gcc --version
    sudo apt-get install build-essential #若报错或等级不够则使用 
    sudo apt install linux-headers-$(uname -r)  

#### 3.安装系统推荐的GPU drive；  
  Ubuntu系统设置 --> 软件和更新 --> 附加驱动 --> 选择最新的NVIDIA drive。  
  
  如果按照成功，用 nvidia-smi指令，有如下图所示内容出现。

  
#### 4.根据GPU drive 匹配 CUDA；  
【版本不对后续会出问题】
i[image]()  

#### 5.根据CUDA vision 匹配 kernel vision；  
【版本不对后续会出问题】
i[image]()  

  
#### 6.（可选）当选择出的 kernel vision ≠ 系统本身的kernel vision  --> 安装特定kernel 并 替换 kernel；  
  
  ##### 1) 查看可更新版本：
    sudo apt-cache search linux-image
    
  ##### 2）安装指定版本：  
    sudo apt-get install linux-image-x.x.x-x-generic；  
    sudo apt-get install linux-image-extra-x.x.x-x-generic；   
    sudo apt-get install linux-headers-x.x.x-x-generic。  
    
  ##### 3）修改grub文件：【便于进入grub界面】  
    sudo vim /etc/default/grub;  
    i[image]()  
      
      
  ##### 4)后续删除多余版本kernel，即可改回grub。

##### 7.关闭 nouveau:
    cat /etc/modprode.d/blacklist-nouveau.conf #创建nouveau黑名单
    blacklist nouveau #写入如下两句 
    options nouveau modeset=0 #保存退出
    sudo update-initramfs -u
    sudo reboot #重启
    lsmod | grep nouveau #没有打印内容则成功
    
#### 8.下载并安装版本对应的CUDA，并设置好环境变量；  
  
#### 9.下载并安装版本对应的cuDNN
