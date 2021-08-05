---
title: 30 分钟部署一个 Kubernetes 集群（转载自 - 阿良)
tags: Kubernetes
abbrlink: 25f9b19
date: '2021-08-06 00:59'
categories: cicd
---

kubeadm 是官方社区推出的一个用于快速部署 kubernetes 集群的工具。

这个工具能通过两条指令完成一个 kubernetes 集群的部署：

```
# 创建一个 Master 节点
$ kubeadm init

# 将一个 Node 节点加入到当前集群中
$ kubeadm join <Master节点的IP和端口 >
```

## **1. 安装要求**

在开始之前，部署 Kubernetes 集群机器需要满足以下几个条件：

- 一台或多台机器，操作系统 CentOS7.x-86_x64
- 硬件配置：2GB 或更多 RAM，2 个 CPU 或更多 CPU，硬盘 30GB 或更多
- 集群中所有机器之间网络互通
- 可以访问外网，需要拉取镜像
- 禁止 swap 分区

## **2. 学习目标**

1. 在所有节点上安装 Docker 和 kubeadm
2. 部署 Kubernetes Master
3. 部署容器网络插件
4. 部署 Kubernetes Node，将节点加入 Kubernetes 集群中
5. 部署 Dashboard Web 页面，可视化查看 Kubernetes 资源

## **3. 准备环境**

![https://blog-1252881505.cos.ap-beijing.myqcloud.com/k8s/single-master.jpg](https://blog-1252881505.cos.ap-beijing.myqcloud.com/k8s/single-master.jpg)

[Untitled](https://www.notion.so/476a92d26dba47c0aa0f30ee70244221)

```
关闭防火墙：
$ systemctl stop firewalld
$ systemctl disable firewalld

关闭selinux：
$ sed -i 's/enforcing/disabled/' /etc/selinux/config  # 永久
$ setenforce 0  # 临时

关闭swap：
$ swapoff -a  # 临时
$ vim /etc/fstab  # 永久

设置主机名：
$ hostnamectl set-hostname <hostname>

在master添加hosts：
$ cat >> /etc/hosts << EOF
192.168.31.61 k8s-master
192.168.31.62 k8s-node1
192.168.31.63 k8s-node2
EOF

将桥接的IPv4流量传递到iptables的链：
$ cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
$ sysctl --system  # 生效

时间同步：
$ yum install ntpdate -y
$ ntpdate time.windows.com
```

## **4. 所有节点安装 Docker/kubeadm/kubelet**

Kubernetes 默认 CRI（容器运行时）为 Docker，因此先安装 Docker。

### 4.1 安装 Docker

```
$ wget <https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo> -O /etc/yum.repos.d/docker-ce.repo
$ yum -y install docker-ce-18.06.1.ce-3.el7
$ systemctl enable docker && systemctl start docker
$ docker --version
Docker version 18.06.1-ce, build e68fc7a
# cat > /etc/docker/daemon.json << EOF
{
  "registry-mirrors": ["<https://b9pmyelo.mirror.aliyuncs.com>"]
}
EOF
```

### 4.2 添加阿里云 YUM 软件源

```
$ cat > /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg <https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg>
EOF
```

### 4.3 安装 kubeadm，kubelet 和 kubectl

由于版本更新频繁，这里指定版本号部署：

```
$ yum install -y kubelet-1.17.0 kubeadm-1.17.0 kubectl-1.17.0
$ systemctl enable kubelet
```

## **5. 部署 Kubernetes Master**

在 192.168.31.61（Master）执行。

```
$ kubeadm init \\
  --apiserver-advertise-address=192.168.31.61 \\
  --image-repository registry.aliyuncs.com/google_containers \\
  --kubernetes-version v1.17.0 \\
  --service-cidr=10.96.0.0/12 \\
  --pod-network-cidr=10.244.0.0/16
```

由于默认拉取镜像地址 [k8s.gcr.io](http://k8s.gcr.io) 国内无法访问，这里指定阿里云镜像仓库地址。

使用 kubectl 工具：

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
$ kubectl get nodes
```

## **6. 安装 Pod 网络插件（CNI）**

```
$ kubectl apply -f <https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml>
```

确保能够访问到 [quay.io](http://quay.io) 这个 registery。

如果 Pod 镜像下载失败，可以改成这个镜像地址：lizhenliang/flannel:v0.11.0-amd64

## **7. 加入 Kubernetes Node**

在 192.168.31.62/63（Node）执行。

向集群添加新节点，执行在 kubeadm init 输出的 kubeadm join 命令：

```
$ kubeadm join 192.168.31.61:6443 --token esce21.q6hetwm8si29qxwn \\
    --discovery-token-ca-cert-hash sha256:00603a05805807501d7181c3d60b478788408cfe6cedefedb1f97569708be9c5
```

## **8. 测试 kubernetes 集群**

在 Kubernetes 集群中创建一个 pod，验证是否正常运行：

```
$ kubectl create deployment nginx --image=nginx
$ kubectl expose deployment nginx --port=80 --type=NodePort
$ kubectl get pod,svc
```

访问地址：http://NodeIP:Port

## **9. 部署 Dashboard**

```
$ kubectl apply -f <https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml>
```

默认 Dashboard 只能集群内部访问，修改 Service 为 NodePort 类型，暴露到外部：

```
kind: Service
apiVersion: v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
spec:
  ports:
    - port: 443
      targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
```

访问地址：http://NodeIP:30001

创建 service account 并绑定默认 cluster-admin 管理员集群角色：

```
kubectl create serviceaccount dashboard-admin -n kube-system
kubectl create clusterrolebinding dashboard-admin --clusterrole=cluster-admin --serviceaccount=kube-system:dashboard-admin
kubectl describe secrets -n kube-system $(kubectl -n kube-system get secret | awk '/dashboard-admin/{print $1}')
```

使用输出的 token 登录 Dashboard。

