---
layout: post
title:  "宝塔面板一键docker部署 by pch18"
categories: docker
tags: baota docker bt-panel
author: Ripncn
---

* content
{:toc}

#宝塔面板一键docker部署

镜像为绑定github的dockerfile文件在dockerHub上自动生成,所以不可能有植入后台的行为,请放心使用.
镜像生成文件可以在github上查看.请大家监督.
制作这个纯粹是为了赚点github的关注量,好用之余请不要忘了去github加个Star一下哦,链接在最下方

##通过host模式运行宝塔镜像
>docker run -tid --name baota --net=host --privileged=true --shm-size=1g --restart always -v ~/wwwroot:/www/wwwroot pch18/baota

建议使用上述host网络模式启动,不需要设置映射端口,自动映射宝塔面板全端口到外网
正常的bridge模式可能会造成网站后台不能获取用户真实ip地址.

##通过bridge模式运行宝塔镜像
如果特殊情况不能使用host网络模式(macos和windows不支持host), 使用下述命令重新以bridge网络模式运行

>docker run -tid --name baota -p 80:80 -p 443:443 -p 8888:8888 -p 888:888 --privileged=true --shm-size=1g --restart always -v ~/wwwroot:/www/wwwroot pch18/baota

##登录方式
登陆地址 http://{{面板ip地址}}:8888
初始账号 username
初始密码 password
由于docker镜像的特殊性，随机密码是安装面板的时候生成的，
所有用户的随机密码其实都相同，没有随机的意义，
为了方便部署，已经去除安全入口，且设置成上述密码，
请大家登陆后第一时间修改账号密码！！

##删除容器命令如下
>docker rm -fv baota

##版本命名说明
pch18/baota或pch18/baota:latest等同pch18/baota:lnmp
pch18/baota:lnmp为最新版本的官方纯净安装的基础上安装nginx,mysql,php
pch18/baota:lnp 为官方版本纯净安装的基础上安装nginx,php(不内置mysql,用于外置数据库的环境)
pch18/baota:lamp 为官方版本纯净安装的基础上安装apache,php
pch18/baota:lap 为官方版本纯净安装的基础上安装apache,php(不内置mysql,用于外置数据库的环境)
pch18/baota:clear 为官方版本纯净安装, 不默认安装nginx,mysql,php等程序

/www文件夹建议保存在volume卷中, /www/wwwroot建议映射到宿主机的目录下,方便上传网站代码等文件

安装完成后以后可以随时使用内置升级,升级到最新版本,
由于面板数据都保存在持久化的卷中, 即使删除容器(不删除volumn)后重新运行,
原来的面板和网站数据都能得到保留.
启动容器时自动启动所有服务

如果还没有安装docker的请运行这个安装脚本
https://pch18.cn/archives/install_docker.html

好用请收藏加星支持一下,谢谢! 其他问题和建议请在github的issue里面交流.
github issue传送门: https://github.com/pch18-docker/baota/issues
dockerHub传送门: https://hub.docker.com/r/pch18/baota/
个人主页传送门: https://pch18.cn/archives/docker-baota.html
