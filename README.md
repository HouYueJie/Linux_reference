# Linux_reference
关于linux的相关操作参考

# 一.在Ubuntu16 上安装CUDA8 【单系统方法】

## - 配置环境  
Ubuntu 16.04 <-> kernel vision 4.4.0.31  
NVIDIA GPU dirve 384.130  
CUDA Toolkit 8.0(GA2)  
cuDNN 8.0-linux-x64-v7.1  
  
##  -安装过程  
### 预备依赖库
    sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
    sudo apt-get install --no-install-recommends libboost-all-dev
    sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev 
    sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
    sudo apt-get install git cmake build-essential
    
### 1.确定系统 kernel vision 和 system vision；  
![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/1.png)    

### 2.查看gcc 和 g++ 版本情况:  
    sudo gcc --version
    sudo g++ --version
    #若报错则使用下面两条语句 
    sudo apt-get install build-essential 
    sudo apt install linux-headers-$(uname -r)    
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/2.png)
    #若没有安装或者等级太高【自行百度降级流程】
    
### 3.安装系统推荐的GPU drive；  
  Ubuntu系统设置 --> 软件和更新 --> 附加驱动 --> 选择最新的NVIDIA drive。    
![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/3.jpg)  
  如果按照成功，用 nvidia-smi指令，有如下图所示内容出现。
![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/4.png)

### 4.根据GPU drive 匹配 CUDA vision；  
【版本不对后续会出问题!!!】  
 ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/5.png)  

### 5.根据CUDA vision 匹配 kernel vision；  
【版本不对后续会出问题!!!】  
 ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/6.png)  

  
### 6.（可选）当选择出的 kernel vision ≠ 系统本身的kernel vision  --> 安装特定kernel 并 替换 kernel；  
   #### > 1 查看可更新版本：
    sudo apt-cache search linux-image
    
   #### >2 安装指定版本：  
    sudo apt-get install linux-image-x.x.x-x-generic；  
    sudo apt-get install linux-image-extra-x.x.x-x-generic；   
    sudo apt-get install linux-headers-x.x.x-x-generic   
    
   #### >3 修改grub文件：【便于重启后进入grub界面】  
    sudo vim /etc/default/grub;  
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/7.png)  
   #### >4 在高级选项中选择合适的内核启动  
   
   #### >5 后续删除多余版本kernel，即可改回grub。

### 7.关闭 nouveau:
    cat /etc/modprode.d/blacklist-nouveau.conf #创建nouveau黑名单
     * blacklist nouveau #写入如下两句 
     * options nouveau modeset=0 #保存退出
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/8.png)   
    sudo update-initramfs -u
    sudo reboot #重启
    lsmod | grep nouveau #没有打印内容则成功
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/9.png)  
### 8.下载并安装版本对应的CUDA，并设置好环境变量；  
   #### >1 下载
    https://developer.nvidia.com/cuda-toolkit-archive
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/10.png)
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/12.png)
   【安装前阅读Online Documentation 中的 Guide ！！！】  
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/11.png)
   #### >2 安装
    sudo sh cuda_xxx_xxx_linux.run --no-opengl-libs
    【默认安装路径、除安装NVIDIA外，其余步骤都是输入‘y’，没有则按enter键】
    
   #### >3 配置环境变量
    sudo vim ~/.bashrc
     * export PATH=/usr/local/cuda-9.1/bin:$PATH
     * export LD_LIBRARY_PATH=/usr/local/cuda-9.1/lib64:$LD_LIBRARY_PATH
    sudo vim /etc/profile #动态链接库
     * export PATH=/usr/local/cuda/bin:$PATH
    sudo vim /etc/ld.so.conf.d/cuda.conf #创建连接文件
     * /usr/local/cuda/lib64
    sudo ldconfig #执行
    
   #### >4 验证安装成功
    cd /usr/local/cuda-*/sample/1_Utilities/deviceQuery
    sudo make
    sudo ./deviceQuery
    有如下图所示结果，则安装成功
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/13.png)
  
### 9.下载并安装版本对应的cuDNN
   #### >1 下载 【推荐解压包安装，稳定】
    https://developer.nvidia.com/rdp/cudnn-archive
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/14.png)  
    
   #### >2 解压
    tar -xzvf cudnn-9.0-linux-x64-v7.tgz
    
   #### >3 拷贝头文件和库文件并给予权限
    sudo cp cuda/include/cudnn.h /usr/local/cuda/include
    sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
    sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
    
   #### >4 验证安装成功
    cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
    有如下图所示结果，则安装成功
   ![image](https://github.com/HouYueJie/Linux_reference/blob/main/CUDA_IMG/15.png)

