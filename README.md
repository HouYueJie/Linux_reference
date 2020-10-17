# Linux_reference
关于linux的相关操作参考

# 一.在Ubuntu上安装CUDA
- 配置环境  
Ubuntu 16.04 <-> kernel vision 4.4.0.31  
NVIDIA GPU dirve 384.130  
CUDA Toolkit 8.0(GA2)  
cuDNN 8.0-linux-x64-v7.1  
  
-安装过程  
1.确定系统 kernel vision 和 system vision；  
![image](https://github.com/HouYueJie/Linux_reference/blob/master/CUDA_IMG/1.png)  
2.查看gcc 和 kernel header 和 package development的版本情况；  
  
3.安装系统推荐的GPU drive；  
  
4.根据GPU drive 选择合适的 CUDA；  
  
5.根据CUDA vision 选择合适的 kernel vision；  
  
6.（可选）当选择出的 kernel vision ≠ 系统本身的kernel vision  --> 安装特定kernel 并 替换 kernel；  
  
7.关闭 nouveau；  
  
8.下载并安装版本对应的CUDA，并设置好环境变量；  
  
9.下载并安装版本对应的cuDNN
