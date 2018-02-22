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
**原因是cudnn的版本太低**
下载新版本的cudnn配置