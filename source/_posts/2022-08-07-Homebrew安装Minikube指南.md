---
title: Homebrew安装Minikube指南
date: 2022-08-07 08:30:53
categories:
- Homebrew
- Minikube
- Kubernetes
tags:
- Homebrew
- Minikube
- Kubernetes

---

### 1. 安装Homebrew

**由于国内网络环境问题，Homebrew官方脚本可能会安装失败，可采用以下国内源安装使用**

#### (1) 苹果电脑标准安装脚本：（推荐 优点全面 缺点慢一点）

```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```
#### (2) 苹果电脑极速安装脚本：（优点安装速度快 缺点update功能需要命令修复）
```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)" speed
```
#### (3) Linux 标准安装脚本：
```
rm Homebrew.sh ; wget https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh ; bash Homebrew.sh
```

#### (4)苹果电脑卸载脚本：
```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```
#### (5)Linux卸载脚本：
```
rm HomebrewUninstall.sh ; wget https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh ; bash HomebrewUninstall.sh
```
[详细使用可点击查看](https://gitee.com/cunkai/HomebrewCN.git)

### 2. 安装minikube

~~~
brew install minikube
~~~

### 3. 启动minikube

~~~
minikube start --driver=docker --kubernetes-version=1.22.1 --image-repository='registry.cn-hangzhou.aliyuncs.com/google_containers'
~~~
启动成功后，如下图所示

![启动成功截图](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081219515.png)

- –-driver 指定驱动为docker，同时也支持VirtualBox、SSH等，具体配置信息可参考[minikube官网](https://minikube.sigs.k8s.io/docs/drivers/)

- --kubernetes-version 指定kubernetes的版本号
- --image-repository 配置镜像源，由于网络问题，可以配置阿里云镜像

### 4. 操作集群

~~~
kubectl get nodes
kubectl get pods -A
~~~
还可以通过将以下内容添加到shell配置中来使用
~~~
alias kubectl="minikube kubectl --"
~~~
最初，某些服务（例如 storage-provisioner）可能尚未处于运行状态。这是集群启动期间的正常情况，并且会立即自行解决。为了进一步了解集群状态，minikube 捆绑了 Kubernetes Dashboard：

~~~
minikube dashboard
~~~

### 5. 管理集群
在不影响已部署应用程序的情况下暂停 Kubernetes：
```
minikube pause
```
取消暂停暂停的实例：
```
minikube unpause
```
停止集群：
```
minikube stop
```
增加默认内存限制（需要重新启动）：
```
minikube config set memory 16384
```
浏览易于安装的 Kubernetes 服务目录：
```
minikube addons list
```
创建第二个运行旧 Kubernetes 版本的集群：
```
minikube start -p aged --kubernetes-version=v1.16.1
```
删除所有 minikube 集群：

```
minikube delete --all
```

### 6. 完全卸载minikube

~~~
minikube stop
minikube delete &&
docker stop $(docker ps -aq) &&
rm -rf ~/.kube ~/.minikube &&
sudo rm -rf /usr/local/bin/localkube /usr/local/bin/minikube &&
launchctl stop '*kubelet*.mount' &&
launchctl stop localkube.service &&
launchctl disable localkube.service &&
sudo rm -rf /etc/kubernetes/ &&
docker system prune -af --volumes
brew uninstall minikube
~~~

**各位看官老爷觉得对自己有帮助，麻烦三连走起哦，如有错误之处欢迎各位指正!**
