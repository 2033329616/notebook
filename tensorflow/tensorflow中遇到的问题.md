---
title: tensorflow中遇到的问题 
tags: tensorflow,python
grammar_cjkRuby: true
---
2018-2-22

[toc]

### **1.CuDNN和CUDA版本的兼容问题** 
提示错误如下:
Loaded runtime CuDNN library: 7003 (compatibility version 7000) but source was compiled with 6021 (compatibility version 6000).  If using a binary install, upgrade your CuDNN library to match.  If building from sources, make sure the library loaded at runtime matches a compatible version specified during compile configuration
**原因是cudnn的版本与tensorflow的版本不一致**
这里使用的cuda8.0,cudnn7.0.3和tensorflow1.4,该版本的tensorflow使用cudnn6就可以
下载6.0版本的cudnn并配置
`tar -xzvfcudnn-8.0-linux-x64-v6.0.tgz`解压文件
把解压后的文件拷贝到CUDA Toolkit的目录
`sudo cp cuda/include/cudnn.h /usr/local/cuda/include`
`sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64`
`sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*`