---
title: Git+Jenkins+Harbor+Docker实现CICD 的一些记录
date: 2021.8.8
tags: CICD
abbrlink: '0'
categories: CICD
abbrlink: 
---
## 环境准备:

ctntos8.4  cpu:4 内存: 2048M 硬盘: 256G

192.168.1.102 Harbor

192.168.1.103 Jenkins

192.168.1.104 Docker

## 一些问题:

```bash
# sshd
systemctl start sshd
ps -e | grep sshd
# 修改hostname
vi /etc/hostname
hostnamectl set-hostname appjzw
# ping 不通外网
vim /etc/resolv.conf

nameserver 8.8.8.8
nameserver 202.106.0.20

nmcli c reload enp0s3
```

## Harbor 服务器安装

```bash
wget [<https://github.com/goharbor/harbor/releases/download/v2.3.1/harbor-offline-installer-v2.3.1.tgz>](<https://github.com/goharbor/harbor/releases/download/v2.3.1/harbor-offline-installer-v2.3.1.tgz>)
yum install lrzsz

# 从服务器拉东西
scp root@[公网地址]:/root/harbor-fooline-installer-v2.3.1.tgz /usr/yzhou/Desktop

systemctl start docker
docker -v
docker —version
docker info

docker images
docker ps

cp harbor.yml.tmpl harbor.yml
vim harbor.yml

./install.sh

cd harbor
docker-compose restart
docker-compose down -v
docker-compose up -d
```

![2](Git+Jenkins+Harbor+Docker%E5%AE%9E%E7%8E%B0CICD%20%E7%9A%84%E4%B8%80%E4%BA%9B%E8%AE%B0%E5%BD%95.assets/2.png)

## docker服务器安装

```jsx
yum install -y yum-utils  device-mapper-persistent-data lvm2
docker -v
yum-config-manager  --add-repo   [<https://download.docker.com/linux/centos/docker-ce.repo>](<https://download.docker.com/linux/centos/docker-ce.repo>)
yum install docker-ce docker-ce-cli [containerd.io](<http://containerd.io/>) --nobest
yum install container-selinux
systemctl start docker
yum install jq -y
```

## jenkins服务器安装

```jsx
sudo wget -O /etc/yum.repos.d/jenkins.repo \\
    <https://pkg.jenkins.io/redhat-stable/jenkins.repo>
sudo rpm --import <https://pkg.jenkins.io/redhat-stable/jenkins.io.key>
sudo yum upgrade
sudo yum install jenkins java-1.8.0-openjdk-devel
sudo systemctl daemon-reload
[root@jenkins ~]# wget -o /etc/yum.repos.d/jenkins.repo [<https://pkg.jenkins.io/redhat-stable/jenkins.repo>](<https://pkg.jenkins.io/redhat-stable/jenkins.repo>)
[root@jenkins ~]# rpm --import [<https://pkg.jenkins.io/redhat-stable/jenkins.io.key>](<https://pkg.jenkins.io/redhat-stable/jenkins.io.key>)

yum install -y jenkins git maven
systemctl start jenkins
ps -ef | grep jenkins
```

gcc

yum源没有对应版本包匹配:

```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo <http://mirrors.aliyun.com/repo/Centos-8.repo>
sed -i  's/$releasever/8/g' /etc/yum.repos.d/CentOS-Base.repo
yum repolist
```

![Untitled](Git+Jenkins+Harbor+Docker%E5%AE%9E%E7%8E%B0CICD%20%E7%9A%84%E4%B8%80%E4%BA%9B%E8%AE%B0%E5%BD%95.assets/Untitled.png)

jenkins配置:

插件: maven intergation 和 ssh

配置远程机器

项目构建调试
