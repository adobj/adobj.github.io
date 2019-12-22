---
title: hexo搭建教程
date: 2019-12-22 19:43:58
tags:
---

	本教程使用hexo+hexo-theme-yilia搭建个人静态博客，发布在Github Pages。
	环境：Node.js、Git
	
<!--more-->

## 环境搭建
#### 安装Git客户端

#### 安装Node.js
	
&nbsp;&nbsp;&nbsp;&nbsp;访问官网，根据个人环境选择相应版本，默认安装就可以了。[官方网站](https://nodejs.org/en/download/)

![Node官方图片](/images/hexo/NodeDownloads.png)

&nbsp;&nbsp;&nbsp;&nbsp;安装完毕后在命令行输入以下命令测试是否安装成功，正确会出现版本号。
```
npm -v
node -v
```
#### 配置环境变量

1) 配置npm的全局模块的存放路径及cache的路径，例如两个文件夹放在NodeJS的主目录下，便在NodeJs下建立"node_global"及"node_cache"两个文件夹，输入以下命令改变npm配置
![Node官方图片](/images/hexo/NodeConfig.png)
```
npm config set prefix "D:\services\node-v10.16.0-win-x64\node_global"
npm config set cache "D:\services\node-v10.16.0-win-x64\node_cache"
```
2) 在系统环境变量添加系统变量NODE_PATH，输入路径D:\services\node-v10.16.0-win-x64\node_global\node_modules，此后所安装的模块都会安装到该路径下。 
![Node官方图片](/images/hexo/config_2.png)
3) 在命令行输入以下命令试着安装express（注：“-g”参数指装到global目录下，即“D:\services\node-v10.16.0-win-x64\node_global\node_modules”里面。）
![Node官方图片](/images/hexo/config_3.png)

#### 由于国内访问官方npm源速度较慢，可能出现下载失败得问题，故此处修改为淘宝镜像源。
```
// 设置 淘宝镜像源
npm config set registry https://registry.npm.taobao.org

// 查看 使用的 镜像源
npm config get registry

// 安装 淘宝镜像源 
// 此步骤可省略
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
&nbsp;&nbsp;&nbsp;&nbsp;至此，所有搭建博客需要的本地环境已基本完成

## hexo简单入门
#### 安装hexo及hexo-deployer-git
&nbsp;&nbsp;&nbsp;&nbsp;选择合适的目录，新建文件夹并重命名为自己喜欢的名称（如：我的文件夹为：D:\project\blog\docs），博客源码将储存在此处。在该文件夹下右键鼠标，点击 Git Bash Here，输入以下 npm 命令即可安装。
```
//安装hexo
$ npm install hexo-cli -g  
//安装hexo部署到Git仓库所依赖的插件
$ npm install hexo-deployer-git --save
```
#### 初始化hexo博客并安装设置模板
```
//初始化博客
$ hexo init 
//关闭bash，进入目录themes，右键鼠标，点击Git Bash Here，执行命令
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
//修改hexo根目录下的_config.yml 
theme: yilia
```

#### 本地启动博客并发布到Github Pages
```
//清空历史生成文件
$ hexo clean
//生成静态页
$ hexo generate  
//启动服务
$ hexo server  
```
博客根目录如下：
![Node官方图片](/images/hexo/config_4.png)
themes目录如下：
![Node官方图片](/images/hexo/config_5.png)
生成静态文件：
![Node官方图片](/images/hexo/config_6.png)
启动服务：
![Node官方图片](/images/hexo/config_7.png)
打开浏览器访问 http://localhost:4000 ，即可看到博客已经成功在本地运行了。

尚需完善模块记录
- Git客户端安装
- 发布博客到远程仓库
- Github Pages配置
- 每次发布博客后同步源码备份到dev分支设置
- hexo写博客基本操作(Markdown基本语法)

版权声明：本文为博主「adobj」的原创文章，转载请附上原文出处链接及本声明。
[原文链接](https://www.2766969.xyz/)