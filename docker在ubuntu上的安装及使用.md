---
title: docker在ubuntu上的安装及使用 
tags: 新建,
grammar_cjkRuby: true
---
2018-3-30
[TOC]
#### **1.安装前的准备工作**
 1. 确保是64位的操作系统
 2. 卸载旧版本的docker
`sudo apt-get remove docker docker-engine docker.io`
dockerde相关文件都在`/var/lib/docker/`路径下
#### **2.使用docker仓库安装**
> 首次安装docker时需要添加docker的仓库

 1. 更新软件源的仓库信息
 `sudo apt-get update`
 2. 安装包来使apt通过HTTPS来使用docker仓库
 `sudo apt-get install apt-transport-https ca-certificates \`
`curl software-properties-common`	
3.添加docker的官方GPG
