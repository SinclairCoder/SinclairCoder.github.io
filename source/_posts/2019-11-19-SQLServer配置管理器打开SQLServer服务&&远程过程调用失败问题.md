---

title:  SQLServer配置管理器打开SQLServer服务&&远程过程调用失败问题   
date: 2019-11-19
author: SinclairWang
# img: 
top: false # 是否推荐
cover: false # 是否轮播
# password: 
toc: false
mathjax: false
# img: /source/images/xxx.jpg
tags:
    - SQLServer
categories:
	- DataBase
---




## 打开SQL Server配置管理器
在Windows 10中，SQL Server 配置管理器不显示为一个应用程序。 
如果想找到配置管理器一般在下面这个路径里

```bash
C:\Windows\SysWOW64
```

里面文件很多，然后搜索SQLServerManager
![搜索SQLServerManager](https://img-blog.csdnimg.cn/20191119132859931.png)
可能版本不一样，只要是这个就可以，然后双击打开。
![配置管理器](https://img-blog.csdnimg.cn/20191119133753870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NpbmNsYWlyV2FuZw==,size_16,color_FFFFFF,t_70)


## 远程过程调用失败问题

打开服务的时候可能会遇到无法连接到 WMI 提供程序的问题
![无法连接到 WMI 提供程序的问题](https://img-blog.csdnimg.cn/20191119134055201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NpbmNsYWlyV2FuZw==,size_16,color_FFFFFF,t_70)

八成是因为之前装过VS，然后装上了Microsoft SQL Server 201* Express LocalDB（版本可能不一样），可以去控制面板添加或卸载程序 里面搜索Microsoft SQL Server 就可以看到，然后卸载掉应该就可以了。