---
title: 一步一步教你如何部署OLAINDEX
date: 2020-04-09 21:02:27
categories:
- 各种有趣的折腾
---
相信不少小伙伴一定听过onedrive，这款微软家的网盘。但由于它主要是一种同步类的网盘，想要像百度云盘那样快捷的分享网盘中的资源就变得很困难。你可能有听过onedrive,但今天我想介绍一下另一个更加强大的应用（当然也更难部署），请容我向你介绍今天的主角——[OLAINDEX](https://github.com/WangNingkai/OLAINDEX/)。
<!--more-->
具体的介绍都在项目的GitHub仓库里了，今天的重点是如何去部署（~~不去踩坑~~），我将一步一步地（尽可能每一步都有截图，尽量做到让小白也能部署），请小伙伴们保有一定的耐心。

示例地址：[HIHIA的云分享](https://cloud.hihia.top)

## 开始前的准备
- 需要一台云服务器，配置不需要多高（大概是CPU一核，内存尽量1G，内存不足可能会失败）。
- onedrive账号一枚~（世纪互联或者国际版的都行，但我这里是国际版）
- 较为基础的linux知识，有云服务器配置基础（我都会尽量多图演示）
- 耐心耐心耐心（重要的话说三遍）

## 部署流程
### 安装前提
云服务器的系统我这里用的是亚马逊AWS云的服务器，当然也可以选诸如阿里云、腾讯云、华为云等。系统可选CentOS 6.x / 7.x (Ubuntu、Debian)，确保是干净的操作系统，没有安装过其它环境带的Apache/Nginx/php/MySQL（已有环境不可安装）我这里演示的是Ubuntu Server 18.04 LTS下的操作。

关于云服务器端口的开放问题请参考以下：
- 腾讯云：https://www.bt.cn/bbs/thread-1229-1-1.html
- 阿里云：https://www.bt.cn/bbs/thread-2897-1-1.html
- 华为云：https://www.bt.cn/bbs/thread-3923-1-1.html

### 安装宝塔面板
```shell
#Centos安装命令：
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh

#Ubuntu/Deepin安装命令：
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh

#Debian安装命令：
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh

#Fedora安装命令:
wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh
```
安装完成后

![](https://image.hihia.top/Screenshot/20200409211518.png)

上图中红框框住的地方分别是宝塔面板的访问地址，用户名和密码，在浏览器访问地址，输入用户名和密码便可进入宝塔面板，进入后安装套件LNMP，注意PHP版本要选7.2，然后点一键安装即可。

![](https://image.hihia.top/Screenshot/20200409211811.png)

还是挺快的~

![](https://image.hihia.top/Screenshot/20200409211939.png)

接下来我们要安装PHP拓展，在页面左边找到软件商店，在右侧找到我们刚才安装的PHP7.2,点击设置

![](https://image.hihia.top/Screenshot/20200409212238.png)

在安装拓展中找到fileinfo和opcache，服务器内存小于1G的真的装不上fileinfo(~~别问我怎么知道的~~)

![](https://image.hihia.top/Screenshot/20200409212346.png)

安装也是很快的~

![](https://image.hihia.top/Screenshot/20200409212612.png)

安装完成之后我们需要禁用几个函数，在禁用函数里找到**exec**、**shell_exec**、**proc_open**和**putenv**。部分教程里可能提到还有**proc_get_status**，但是这个并不在里面，并且实测没有影响。

![](https://image.hihia.top/Screenshot/20200409212710.png)

接下来我们需要修改composer源，宝塔面板默认安装了 Composer 包管理器，进入ssh命令行，执行以下命令更换为国内源（由于我的服务器在国外所以没有执行）：

```shell
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```

到这里基本的PHP环境就配置好了。

这时重启PHP服务即可

![](https://image.hihia.top/Screenshot/20200409220205.png)

### 安装配置

在左侧找到网站，点击添加站点，输入域名并选择PHP版本为7.2，其他的不动，直接提交即可。

这里的域名可以写你的主域名，也可以写你的二级域名，我这里用的就是我博客域名的二级域名

![](https://image.hihia.top/Screenshot/20200409213716.png)

我的域名是在阿里云解析的，可参看如下解析配置，记录值便是你服务器的公网ip

![image.png](https://image.hihia.top/blog/8410a9c490fe922010d124894fc5cea1.png)

接下来我们就可以开始初始化和安装olaindex了，在ssh面板中一条一条执行一下内容(因为Ubuntu server禁止了root用户，故我这里的操作都加了sudo)：
```shell
cd 站点目录 # 就是你刚才添加站点时的根目录，我这里是/www/wwwroot/cloud.hihia.top
git clone https://github.com/WangNingkai/OLAINDEX.git tmp 
sudo mv tmp/.git . 
sudo rm -rf tmp 
git reset --hard 
sudo cp database/database.sample.sqlite database/database.sqlite  # 数据库文件
sudo composer install -vvv
sudo chmod -R 777 storage/	#部分教程中这里是755，之后会出现错误
sudo chown -R www:www *
sudo php artisan od:install
```
最后一条执行完了便如下操作，域名填写你刚才新建站点时的域名就行，顺利的话我们就完成了olaindex的安装：

![](https://image.hihia.top/Screenshot/20200409214946.png)

之后我们来配置站点信息~，回到刚才添加站点那里，点设置。

![](https://image.hihia.top/Screenshot/20200409215228.png)

进到网站目录，取消防跨站勾选框，运行目录选择/public,最后点击右边的保存即可。

![](https://image.hihia.top/Screenshot/20200409215349.png)

接下来进到伪静态，选择laravel5，点击保存。

![](https://image.hihia.top/Screenshot/20200409215558.png)

接下来再进到配置文件，注释掉框住的部分（这里的行数可能跟你现在看到的不同，因为后面还有配置SSL所以我这里的配置文件多了一些行，但是注释的内容是相同的），这里注释是为了防止图片404，点击保存即可。

![](https://image.hihia.top/Screenshot/20200409215739.png)

接下来配置SSL，做这步之前要确保你的域名已经解析。这里操作当时忘截图了，大概是这样（我配置的时候并没有让我填管理员邮箱）申请成功后点击右上角的强制HTTPS：

![](https://image.hihia.top/Screenshot/20200409220400.png)

### 绑定环节

接下来我们可以开始绑定我们的onedrive到olaindex上了，在浏览器输入**https://你的域名**，输入账号admin，密码12345678，即可进入API申请页面，我们现在需要client_id和client_secret：

![](https://image.hihia.top/Screenshot/20200409221344.png)

点击申请并登录你的onedrive账号后你可以看到如下图所示，这个便是你onedrive的client_secret：

![](https://image.hihia.top/Screenshot/20200409221007.png)

再点击下面的**知道了，返回快速启动**即可看到如下图所示，这个便是onedrive的client_id了：

![](https://image.hihia.top/Screenshot/20200409221217.png)

返回刚才的页面分别输入client_id和client_secret后，选择你账号的类型点击下方的保存即可，如果顺利的话便可见到如下图所示，点击接受就行：

![](https://image.hihia.top/Screenshot/20200409221635.png)

到此为止我们的配置就结束啦~，在浏览器输入域名即可访问你的olaindex了，至于接下来的配置就是一些基础的设置了，测试了一下的确是不限速的。

### 问题反馈
如果在过程中还有哪些问题的可以到作者项目的issue去提问，也可以在这下方的留言区提问，最后，完结撒花❀。