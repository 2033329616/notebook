---
title: python3学习笔记
tags: 新建,
grammar_cjkRuby: true
---
2017-12-8

[toc]

### 1.Python及第三方库api查看
在终端运行`python –m pydoc –p 1234`
浏览器中运行： [http://localhost:1234/][1]就可以打开python及集成的其他库的api了 .
这里主要使用的就是python自带的pydoc文档生成工具，pydoc具体使用可以参看python文档：https://docs.python.org/2/library/pydoc.html

### 2. pyaudio语音处理模块的安装
安装` sudo apt-get install libjack-jackd2-dev portaudio19-dev`
再运行`sudo python3 -m pip install pyaudio`

### 3. ffmpeg音频格式转换
`sudo apt-get install ffmpeg`安装,
使用`ffmpeg -i 16k.wav 123.wav转换格式`

### 4.百度语音模块SDK的安装

如果已安装pip，执行`pip install baidu-aip`即可。
如果已安装setuptools，执行`python setup.py install`即可。
如果使用python3,需要使用`pip3 install baidu-aip`


  [1]: http://localhost:1234/
  
 