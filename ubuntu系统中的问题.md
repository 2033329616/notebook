---
title: ubuntu系统中的问题
tags: ubuntu系统,
grammar_cjkRuby: true
---
2017-11-17

[toc]
### **1.如何为程序建立桌面快捷方式**
一般在Linux下使用压缩包安装的程序，如小书匠，在解压后的文件夹下有可运行的程序，但需要通过命令行的形式来运行，如果使用桌面快捷方式需要操作如下：
在`/usr/share/applications`目录下创建shortcut_name.desktop文件
```bash
[Desktop Entry]
Version=1.0
Name=Story
Exec=/home/david/Downloads/story/Story-writer %F
Icon=/home/david/Downloads/story/Story-writer.png
Terminal=false
Categories=TextEditor;
StartupNotify=true
Type=Application
Actions=Window;Document;
```
 
 Version：程序的版本号，可以不写
Type：表示类型
Name：图标的名字
Exec：可执行文件所在的绝对路径
Icon：快捷方式的图标使用的图像所在的绝对路径
Terminal：是否在打开程序的时候打开一个终端，通常使用false
其他的按照上述的写法就行

**ctrl + win + d 可以直接显示桌面，再次使用返回当前界面**

### **2.git的使用方法**

 1. git安装和设置
 指令：`sudo apt-get install git` 安装git
 指令：`git config --global user.name "Your Name"`和
 `git config --global user.email "email@example.com"`绑定用户和邮箱
 2. 创建本地仓库
  `mkdir directory`，创建空文件夹，在该文件夹下：
  - `git init`，把这个目录变成Git可以管理的仓库
  -  `git add file2.txt file3.txt`，添加文件到git仓库，该文件只能在该仓库文件夹下
  - `git commit -m "add 2 files."`，把文件提交到仓库，引号中的是说明
 3. 检查当前库的状态
 `git status`查看当前的文件状态
 `git diff`查看文件修改前后的不同状态(可以加单一个文件名，仅查看该文件的信息)
 4. 回退和撤销
 
4.1 回退版本
 `git log` 查看提交的历史日志，包括各个版本的信息和id号，以便确定要回退到哪个版本，使用命令`gitk`可以直接查看版本树
 - HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令 `git reset --hard commit_id`
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本

4.2 撤回修改：
`git checkout -- file` 撤销在工作区的修改
`git reset HEAD file` 撤销`git add file`的操作，撤回暂存区的状态，但工作区的修改还在
- 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`
- 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。
- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

4.3 文件的删除和恢复
从仓库中删除文件
- `rm file` 删除已经提交仓库的文件后，`git status` 可以看到文件的状态是已删除，如果想从仓库中移除该文件使用下面指令：
`git rm file`，`git commit -m "the information about the operation"` 则仓库中将该文件移除，这个过程和添加文件流程一样，同样是1.将工作区文件添加到暂存区，2.提交给仓库
回复要删除的文件

从仓库中恢复文件
- `git checkout -- file`，只有仓库中有该文件才可以恢复

### **3. 使用github托管git的本地仓库**
 
 
 
 
### **4.使用apt-file来查看安装的文件**
`sudo apt-get install apt-file` 安装该软件
安装完以后系统会提示你update，如果没有提示，在终端输入如下命令：
`sudo apt-file update` 更新apt-file
apt-file 是用来查找某个命令或者某一个库所在的包的，具体用法如下：
`apt-file search libz.so.1`
```
lib32z1: /usr/lib32/libz.so.1
lib32z1: /usr/lib32/libz.so.1.2.8
libx32z1: /usr/libx32/libz.so.1
libx32z1: /usr/libx32/libz.so.1.2.8
libzadc1: /lib/x86_64-linux-gnu/genwqe/libz.so.1
zlib1g: /lib/x86_64-linux-gnu/libz.so.1
zlib1g: /lib/x86_64-linux-gnu/libz.so.1.2.8
```
所以使用`sudo apt-get install lib32z1 ` 解决问题