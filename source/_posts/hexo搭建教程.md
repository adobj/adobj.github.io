---
title: hexo搭建教程
abbrlink: 119b
categories: uncategorized
date: 2019-12-22 19:43:58
tags:
- 博客
---

本教程使用hexo+hexo-theme-yilia搭建个人静态博客，发布在Github Pages。
环境：Node.js、Git
	
<!--more-->

## 环境搭建

#### 安装Git客户端

安装[Git for Windows](https://git-scm.com/download/win),创建ssh
```
git config --global user.name "yourname"
git config --global user.email "youremail"
ssh-keygen -t rsa -C "youremail"
```
回车直到结束，在C:\Users\用户名\\.ssh目录下，id_rsa是本地私人密钥，不能公开；id_rsa.pub是公共密钥，把此公钥放在github上。
github配置ssh keys的路径为settings-->SSH and GPG keys.
在gitbash中输入如下命令验证：
```
ssh -T git@github.com
```

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

#### 本地启动博客
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

#### 发布到Github Pages
1.安装git,配置ssh

2.关联
1）安装deploy-git
```
npm install hexo-deployer-git --save
```
2）修改博客根目录_config.yml
```
deploy:
    type: git
    repo: git@github.com:adobj/adobj.github.io.git
    branch: master
```
3）Github Pages配置
新建repository,repository名称尽量与github用户名一致。在仓库的Settings中启用github pages及域名配置。
![Settings](/images/hexo/githubSettings.png)
在repository的根目录创建CNAME文件，文件内容只能有顶级域名。如不绑定域名，此步骤仅需启用github pages即可。
例如：www.example.com
由于deploy时会覆盖所有文件，所以把CNAME文件放置在博客的source文件夹下。
另，source文件夹下的README.md文件在渲染时会自动渲染为html文件。如需要排除渲染此文件，需要在_config.yml文件中增加配置。
```
# 忽略渲染readme文件
skip_render: README.md
```
4）如绑定域名，在域名管理平台增加两条解析。
![Settings](/images/hexo/domain_1.png)
![Settings](/images/hexo/domain_2.png)

3.优化link
1）hexo默认生成链接规则：:year/:month/:day/:title，不够方便简洁，且不利于网站SEO。故使用第三方插件hexo-abbrlink优化。
2）安装插件
```
npm install hexo-abbrlink --save
```
3）修改根目录_config.yml文件
```
permalink: posts/:abbrlink.html  # 此处可以自己设置，也可以直接使用 :/abbrlink
# abbrlink config
abbrlink:
  alg: crc16      #support crc16(default) and crc32
  rep: hex        #support dec(default) and hex
  drafts: false   #(true)Process draft,(false)Do not process draft. false(default) 
  # Generate categories from directory-tree
  # depth: the max_depth of directory-tree you want to generate, should > 0
  auto_category:
     enable: true  #true(default)
     depth:        #3(default)
```
crc16的最大数是65535,个人博客完全够用。
生成链接样例（官方）：
```
crc16 & hex
https://post.zz173.com/posts/66c8.html

crc16 & dec
https://post.zz173.com/posts/65535.html

crc32 & hex
https://post.zz173.com/posts/8ddf18fb.html

crc32 & dec
https://post.zz173.com/posts/1690090958.html
```
4.deploy及源码备份到dev分支
1）博客源码备份原理：通过监听Hexo的事件来完成自动执行Git命令进行自动备份。[hexo事件官方说明文档](https://hexo.io/zh-cn/api/events.html)
2）安装shelljs模块
```
npm install --save shelljs
```
3）在博客根目录的scripts文件夹下新建一个js文件，文件名自定义，本教程采用AutoCommitSourceCode.js。文件中内容为：
```
require('shelljs/global');
try {
    hexo.on('deployAfter', function() {//当deploy完成后执行备份
        run();
    });

} catch (e) {
    console.log("产生了一个错误啊<(￣3￣)> !，错误详情为：" + e.toString());
}
function run() {
    if (!which('git')) {
        echo('Sorry, this script requires git');
        exit(1);
    } else {
        echo("======================Auto Backup Begin===========================");
        cd('D:/project/blog/docs');    //此处修改为Hexo根目录路径
        if (exec('git add --all').code !== 0) {
            echo('Error: Git add failed');
            exit(1);
        }
        if (exec('git commit -am "blog auto backup script\'s commit"').code !== 0) {
            echo('Error: Git commit failed');
            exit(1);
        }
        if (exec('git push origin dev').code !== 0) {
            echo('Error: Git push failed');
            exit(1);
        }
        echo("==================Auto Backup Complete============================")
    }
}
```
***根据个人路径修改，hexo根目录路径及git远程仓库名称***
4）在博客根目录初始化git，并关联github repository的dev分支。
```
git init
git checkout -b dev
# 删除本地master分支，因github repository的master为渲染后的内容，防止误操作。
git branch -D master
# git remote add origin {远程仓库地址}
git remote add origin git@github.com:adobj/adobj.github.io.git
git add .
git commit -m "init dev branch"
git push origin dev
```
5）发布到Github
```
hexo deploy   # 部署
```

版权声明：本文为博主「adobj」的原创文章，转载请附上原文出处链接及本声明。
[原文链接](https://www.2766969.xyz/)