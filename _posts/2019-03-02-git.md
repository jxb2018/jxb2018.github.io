---
layout:     post
title:      GitLab服务器搭建
subtitle:   在CentOS上搭建GitLab服务器
date:       2019-3-2
author:     jxb2018
header-img: img/post-gitlab.jpg
catalog: 	 true
tags:
    - CentOS
    - GitLab
---

## 一：安装并配置必要的依赖关系
> 在CentOS上安装所需的依赖：ssh,防火墙，postfix(用于邮件通知),wget(从外网下载插件)
### 1. 安装ssh
```
sudo yum install -y curl policycoreutils-python openssh-server
```
### 2. 将SSH服务设置成开机自启动
```
sudo systemctl enable sshd
```
### 3. 启动SSH服务
```
sudo systemctl start sshd
```
### 4. 安装防火墙
```
yum install firewalld systemd -y
```
### 5. 开启防火墙
```
service firewalld start
```
### 6. 添加http服务到firewalld
- **pemmanent表示永久生效，若不加--permanent系统下次启动后就会失效。**
```
sudo firewall-cmd --permanent --add-service=http
```
### 7. 重启防火墙
```
sudo systemctl reload firewalld
```
### 8. 安装Postfix以发送通知邮件
```
sudo yum install postfix
```
### 9.将postfix服务设置成开机自启动
```
sudo systemctl enable postfix
```
### 10.启动postfix
```
sudo systemctl start postfix
```
- **在安装Postfix期间，可能会出现配置屏幕。选择“Internet Site”并按enter键。使用您的服务器的外部DNS以“mail name”并按enter。如果出现额外的屏幕，继续按enter键接受默认值。**
### 11.安装wget
```
yum -y install wget
```
## 二、部署GitLab服务器
### 1.添加gitlab镜像
```
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm
```
> 如果下载速度过慢
> 1. 也可以在``` gitlab.com ```下载
> ``` gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm ```
> 2. xshell远程连接虚拟主机，rpm包抛上去

### 2.安装gitlab
```
rpm -i gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm
```
### 3.修改gitlab配置文件指定服务器ip和自定义端口
```
vim  /etc/gitlab/gitlab.rb
```
> external_url 'http:服务器ip:端口'

> ps:注意这里设置的端口不能被占用，默认是8080端口，如果8080已经使用，请自定义其它端口，并在防火墙设置开放相对应得端口
### 4.重置并启动GitLab
```
gitlab-ctl reconfigure
gitlab-ctl restart
```
> 提示  "ok: run:"表示启动成功

### 5.访问GitLab页面
> 如果没有域名，直接输入服务器ip和指定端口进行访问
 
> 初始账户: root 密码:5iveL!fe