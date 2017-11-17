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
 指令：`mkdir directory`创建空文件夹
 指令：`git init`把这个目录变成Git可以管理的仓库
 