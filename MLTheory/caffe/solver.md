# Install

## CUDA

**CUDA**(Compute Unified Device Architecture)，是显卡厂商**NVIDIA**推出的运算平台。 

CUDA™是一种由NVIDIA推出的通用**并行计算架构**，该架构使**GPU**能够解决复杂的计算问题。 

安装CUDA的时候要用NVIDIA包，yum安装的坑多
Install the library and latest driver separately; the driver bundled with the library is usually out-of-date. + CentOS/RHEL/Fedora:

CUDA的下载地址：https://developer.nvidia.com/cuda-downloads。
使用rpm(local)的方式。

使用基础包

```sh
`sudo rpm -i cuda-repo-rhel7-8-0-local-ga2-8.0.61-1.x86_64.rpm`
`sudo yum clean all`
`sudo yum install cuda`
```

再安装：cuda-repo-rhel7-8-0-local-cublas-performance-update-8.0.61-1.x86_64。