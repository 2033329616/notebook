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
 3. 添加docker的官方GPG证书
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
`sudo usermod -aG docker $USER`,其中`$USER`表示当前的用户,再次查看docker用户组的信息如下,该组中多了个david用户(当前登录的用户):
3. 注销登录后,用户组生效,就可以直接使用不带sudo的docker指令

![enter description here][5]

#### **5.安装nvidia-docker来支持GPU**
1. 卸载旧版本的nvidia-docker及GPU容器
`docker volume ls -q -f driver=nvidia-docker |  \`
`xargs -r -I{} -n1 docker ps -q -a -f volume={} | \` 
`xargs -r docker rm -f`
`sudo apt-get purge -y nvidia-docker`
2. 添加仓库
  - 添加key:
`curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -`
  -  获取系统版本号:
 `distribution=$(. /etc/os-release;echo $ID$VERSION_ID)` 
  - 添加仓库:
 `curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \`
  `sudo tee /etc/apt/sources.list.d/nvidia-docker.list` 
 3. 更新仓库信息
 `sudo apt-get update`
4. **安装nvidia-docker2并配置docker dameon加速镜像下载**
 - 安装nvidia的gpu支持:
 `sudo apt-get install -y nvidia-docker2`
 - 修改`/etc/docker/daemon.json`文件内容如下:
 ```json
 {
  "registry-mirrors": ["http://58167a06.m.daocloud.io"],
  "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
 ```
 其中链接部分是使用国内的**加速镜像**,这里使用dalcloud的加速镜像,如果使用阿里云的加速镜像,将链接替换为[https://8vntriz8.mirror.aliyuncs.com][6]即可,该链接是在注册阿里开发者平台时生成的,具体的细节见[https://www.cnblogs.com/atuotuo/p/6264800.html][7].
 
 - 重载daemon使配置生效,并重启docker:
`sudo pkill -SIGHUP dockerd`
`sudo systemctl daemon-reload`
`sudo systemctl restart docker`

5. 测试安装情况
`docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi`会下载cuda镜像来测试

#### **5.使用docker安装tensorflow**
- 打开tensorflow的docker[镜像仓库][8]如下:

![tensorflow镜像][9]
使用指令`nvidia-docker run -it -p 8888:8888 tensorflow/tensorflow:<mirror tag>`下载并运行tag对应的镜像,这里使用tag为1.7.0-gpu-py3的镜像输出结果如下.

![运行tensorflow的镜像][10]
将最下面的链接复制到浏览器就可以打开ipython notebook并使用对应的镜像环境了.

- 注意:首次运行上面的指令会下载对应tag的镜像,之后运行可以直接从本地读取该镜像.

- 使用`docker images`查看已下载镜像信息,结果如下:


![enter description here][11]
- 使用`docker rmi -f <IMAGE ID>` 删除对应ID号的镜像.




  


  [1]: ./images/Screenshot%20from%202018-03-30%2020-41-23.png "目前docker-ce的版本"
  [2]: ./images/Screenshot%20from%202018-03-30%2020-57-04.png "docker安装成功"
  [3]: https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce-1
  [4]: ./images/Screenshot%20from%202018-03-30%2021-31-44.png "docker组的信息"
  [5]: ./images/Screenshot%20from%202018-03-30%2021-34-55.png "david用户加入docker用户组"
  [6]: https://8vntriz8.mirror.aliyuncs.com
  [7]: https://www.cnblogs.com/atuotuo/p/6264800.html
  [8]: https://hub.docker.com/r/tensorflow/tensorflow/tags/
  [9]: ./images/Screenshot%20from%202018-03-31%2014-44-02.png "tensorflow的镜像"
  [10]: ./images/Screenshot%20from%202018-03-31%2014-53-50.png "运行tensorflow的镜像"
  [11]: ./images/Screenshot%20from%202018-03-31%2015-02-54.png "已下载的镜像信息"