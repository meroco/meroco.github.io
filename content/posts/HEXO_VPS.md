---
title: HEXO+VPS 部属流程
date: 2016-07-30 20:06:32
cover: https://i.loli.net/2020/02/20/BGYZeU2AoPzTShk.jpg
categories: 
       - 其它
---


花了将近半个月的时间＝ ＝，走了好多弯路，终于部署成功了，豁然开朗。现在用的是 CONOHA 的东京节点，访问速度应该是非常快速的。下面来简单的写一下部署过程，主要是给自己当个笔记吧，笔者本地计算机的环境是 Mac OS X 10.11.6，虚拟服务器的环境是 CentOS 7.2。

<!-- more -->

---
##### 本地部署  
首先为本地计算机安装 Git 和 Node.js，这部分参考官网的说明即可。安装完成后即可安装 HEXO 博客程序，打开终端输入以下代码。  

``` sh
$ npm install hexo-cli -g     #全局安装
$ hexo init blog     #初始化文件夹
$ npm install    #安装插件
$ hexo generate     #渲染生成静态文件
$ hexo server     #启动本地预览
```

此时打开 [http://localhost:4000/](http://localhost:4000/) 就能看到 "Hello World" 第一篇日志了，本地安装成功。主题和插件自行研究，配置文件是位于 Hexo 文件夹内名为 **_config.yml** 的文件。

--- 
##### VPS 环境  
由于 CONOHA 自动安装了 LAMP，首先要卸载掉 Apache，因为笔者不需要。  

``` sh
$ systemctl stop httpd.service     #停止httpd服务
$ yum remove httpd     #卸载Apache及其依赖包
```

接着安装 Nginx。

``` sh
$ yum install epel-release     #添加CentOS 7依赖包
$ yum install nginx     #安装Nginx
$ systemctl start nginx     #启动Nginx 
$ systemctl enable nginx     #设置开机启动
```

完成上述命令后访问 IP 地址即可看到 Nginx 的默认欢迎页面。
 
---
##### 部属
编辑 Hexo 配置文件 **_config.yml**。  

``` yaml
 deploy:
     type: rsync
     host: xxx.xxx.xxx.xxx     #服务器地址
     user: root
     root: /var/www/hexo     #网站目录
     port: 22
     delete: true
     verbose: true
     ignore_errors: false
```

**_config.yml** 文件对格式的要求极其严格，务必注意空格。可以新建用户，自定义端口，这里为了方便直接使用了 root 用户以及 22 端口。（rsync 同步方式在指定其他端口时会出错，作者至今未修复此 bug，网络上有其他解决方案。）

编辑 Nginx 的配置文件 **nginx.conf**。  

```
server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  misuzu.moe;
        root         /var/www/hexo;
        location / {
        root   /var/www/hexo;
        index index.html index.htm;
        }
｝
```

本机生成静态文件并部属到 VPS。

``` sh
$ cd blog     #进入本地 `blog` 目录
$ hexo clean     #清除生成的静态文件 
$ hexo generate     #生成静态文件
$ hexo deploy     #将静态文件部属到服务器
```
至此部署完毕，完成域名解析并重启 Nginx 后即可通过域名访问网站。

***

最后贴个 [CONOHA](https://www.conoha.jp/referral/?token=pdaWqS2zHILMygFOFOMlEO4TBA2sTf5QkqIr8Ga2xRSy44Royd4-U5I) 的注册链接，如果有朋友想尝试可以通过这个链接注册购买，共利双赢的方式。

**更新：已转移到Github Pages。(2020/02/20)**
