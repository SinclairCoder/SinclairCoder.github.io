---
title:  修改Jupyter Notebook的默认存储路径 
date: 2019-10-2
author: SinclairWang
# img: 
top: false # 是否推荐
cover: false # 是否轮播
# password: 
toc: false
mathjax: false
# img: /source/images/xxx.jpg
tags:
    - JupyterNotebook
categories:
	- Python3
---

最近在复现数据分析，发现JupyterNotebook默认存储路径在C盘 /user/.jupyter里面
想改一下默认存储路径。

## step1
进入Anaconda安装路径，一般来说都是 ***/Anaconda3/Scripts，比如我的就是在 E:\Python\Anaconda\INSTALL\Scripts这个路径下，在cmd中将路径转到该路径下，这样替换代码如下，其中路径替换成自己的。
```bash
>> cd E:\Python\Anaconda\INSTALL\Scripts
>> e:
```
## step2
在cmd中输入命令：
```bash
jupyter notebook --generate-config 
```
然后就会在C盘用户路径（例如我的路径是：C:\Users\三寸旧城七寸执念\\.jupyter）下生成jupyter_notebook_config.py文件，使用文本编辑器（VS Code等）打开该文件。

## step3
定位到第246行
```bash
#c.NotebookApp.notebook_dir = ''
```
替换为
```bash
c.NotebookApp.notebook_dir = 'E:/CodeCache/JupyterNotebook'
```
## step4
右击JupyterNotebook的快捷方式点击属性，将目标那一栏里的%USERPROFILE%删掉，应该就可以了。

重启Jupyter Notebook。

另外，需要给Jupyter NoteBook 换主题，换字体，字号，以及代码自动补全
可参考
[使用 Jupyter Themes 修改 Jupyter Notebook 的主题、字体、字号](https://blog.csdn.net/Jerry_xzj/article/details/89931165)
[Jupyter Notebook 更换主题、设置字体(jupyterthemes的使用)、代码自动补全、变更工作目录（默认目录）](https://blog.csdn.net/az9996/article/details/88621028#_24)

其实个人觉得貌似也没有好看的字体，传说中的Consolas字体貌似也不支持，主题换来换去其实还不如最初的默认的。其实个人觉得比较有用的还是代码补全。
另外，需要注意里面的命令行是在 Anaconda Prompt输入的，而非cmd。