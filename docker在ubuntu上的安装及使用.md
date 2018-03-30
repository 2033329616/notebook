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
> **首次安装docker时需要添加docker的仓库**

 1. 更新软件源的仓库信息
 `sudo apt-get update`
 2. 安装包来使apt通过HTTPS来使用docker仓库
  `sudo apt-get install apt-transport-https ca-certificates \`
  `curl software-properties-common`	
 3. 添加docker的官方GPG
`curl -fsSL https://download.docker.com/linux/ubuntu/gpg \`
`| sudo apt-key add -`
4. 验证是否添加成功带有fingerprint的key
`sudo apt-key fingerprint 0EBFCD88`
5. 添加稳定的docker仓库
`sudo add-apt-repository \`
  ` "deb [arch=amd64] https://download.docker.com/linux/ubuntu \`
  ` $(lsb_release -cs) stable"`
> **安装docker-ce**

1. 再次更新仓库信息
`sudo apt-get update`
2. 指令安装docker
注意:`sudo apt-get install docker-ce`只能安装目前最高的版本,但通常使用下面方法来选择要安装的版本
`apt-cache madison docker-ce`查看目前的多个版本,结果如下:

![目前docker-ce的版本][1]
 使用`sudo apt-get install docker-ce=<VERSION>`安装特定的版本,这里使用17.12.0版本,指令为:`sudo apt-get install docker-ce=17.12.0~ce-0~ubuntu`

3. 验证安装的结果
如果安装docker安装正确,则运行`sudo docker run hello-world`后输出下面结果:

![docker安装成功][2]

- 其他详细的安装详情见官网 [https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce-1][3]


#### **4.创建docker组**
> 该操作的作用:不用每次使用docker指令时都输入sudo
1. 创建docker组
`sudo groupadd docker`创建docker组
`cat /etc/group | grep -i docker`查看docker组的基本情况如下,目前该组中还没有添加用户:

![enter description here][4]
2.为docker组添加用户
`sudo usermod -aG docker $USER`,其中`$USER`表示当前的用户,再次查看docker用户组的信息如下:

![enter description here][5]




















  


  [1]: ./images/Screenshot%20from%202018-03-30%2020-41-23.png "目前docker-ce的版本"
  [2]: ./images/Screenshot%20from%202018-03-30%2020-57-04.png "docker安装成功"
  [3]: https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce-1
  [4]: ./images/Screenshot%20from%202018-03-30%2021-31-44.png "docker组的信息"
  [5]: ./images/Screenshot%20from%202018-03-30%2021-34-55.png "david用户添入docker用户组"