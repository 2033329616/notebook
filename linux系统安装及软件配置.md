---
title: linux系统安装及软件配置 
tags: 系统,开发环境配置
grammar_cjkRuby: true
---
2017-11-29
[TOC]

### 1安装ubunut系统时一直卡在图标的位置无法进入安装界面
将系统bios里security boot设置为disable，重启后再重新进入安装状态；
 如果仍然无法进入安装状态：在选择安装和试用的界面，将光标移动到ubunt上，按 ‘e’，系统进入一个设置界面，在`quiet splash`后面加 `nomodeset`(别忘了空格)，然后按F10后系统以不设置显示模式的状态可以进入安装界面，但此时分辨率较低，尤其在选择分区后无法直接点击“安装”按钮，而且无法点击菜单栏拖动，此时使用**alt + 鼠标左键**可以拖动窗口，然后按步骤安装系统即可。
 
 ### 2电脑安装ubuntu16.04后重启一直黑屏无法进入系统
 可能和电脑的intel核显卡和nvidia集成显卡的驱动有关，此时再以上面的nomodeset模式进入系统，使用指令`sudo apt-get install nvidia-current`，然后重启可以成功进入系统。
       如果没有进入系统又卡在黑屏界面，则使用另一种方法：
      1使用nomodeset模式进入系统，先安装Intel的核心显卡驱动，去官网下载update-tool工具，这是我下载的版本`intel-graphics-update-tool_2.0.2_amd64.deb`，然后安装即可。
    ` sudo dpkg -i intel-graphics-update-tool_2.0.2_amd64.deb `
    ` sudo apt-get -f install`
    ` sudo apt-get update`
安装完成后使用`glxinfo | grep rendering`可以查看安装的结果，显示yes表示安装成功
      2安装nvidia的显卡驱动
     ` lspci | grep -i nvidia`查看电脑是否支持英伟达显卡，安装官网安装驱动
      a关闭图像界面和禁用nouveau
         进入字符界面`ctrl + alt + f1`后登录，`sudo serivce lightdm stop`关闭图像界面服务，禁用nouveau第三方驱动(步骤略)，`lsmod | nouveau`无输出则禁用成功
      b 安装nvidia的deb文件即可
     安装完成后使用`cat /proc/driver/nvidia/version`查看驱动版本，使用`nvidia-smi`也可以查看驱动及显卡的硬件信息
 所有的驱动安装完成后，重启进入系统，如果还没有进入系统，则按上述的办法重安nvidia驱动，使用`sudo apt-get remove nvidia`和`sudo /usr/bin/nvidia-uninstall`将驱动卸载干净再重新安装。
 ### 配置sublime + python3的运行环境
 A   新建运行环境使用Tools==>Build System==>New Build Systems生成一个配置文件，我们可以改   名字为python3.sublime-build，python3就成为了build system中的一个选项了，在该文件中写入：
 ```bash
{
"shell_cmd": "python3 -u \"$file\"",

"file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",

"selector": "source.python",
}
```
保存后在build system中选择python3，使用ctrl + B就可以运行代码了
import sys
print(sys.version)
print(sys.version_info)
上面的代码可以输出使用的python版本，python2版本可以直接选择Tools==>build system中的python就行，应为在系统中默认的是python2，所以这里要构建python3的运行环境。
B  在sublime中加入anaconda的插件，特别好使
先安装package control，然后安装anaconda的插件，点击Preferences==>Package Settings==>Anaconda==>Settings User生成文件Anaconda.sublime-settings，里面写入下面代码：