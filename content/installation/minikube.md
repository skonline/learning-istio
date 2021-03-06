---
date: 2018-09-29T20:00:00+08:00
title: minikube安装
weight: 202
menu:
  main:
    parent: "installation"
keywords:
- minikube安装istio
description : "使用minikube安装istio"
---

## 准备Minikube

安装最新版本的 Minikube。

详细见[Minikube安装](https://skyao.gitbooks.io/learning-kubernetes/installation/minikube.html)

启动minikube的命令：

```bash
minikube start --docker-env http_proxy=http://192.168.31.152:8123 --docker-env https_proxy=http://192.168.31.152:8123 --docker-env no_proxy=localhost,127.0.0.1,::1,192.168.31.0/24,192.168.99.0/24
```

## 准备istio

参考：

- http://istio.doczh.cn/docs/setup/kubernetes/quick-start.html

在linux上执行以下命令：

```bash
curl -L https://git.io/getLatestIstio | sh -
sudo mv istio-0.2.12/ /usr/local/share/istio
```

修改`/etc/profile`，加入一下内容：

```bash
# istio
export PATH=/usr/local/share/istio/bin:$PATH
```

执行`source /etc/profile`载入profile。

```bash
$ istioctl version
Version: 0.2.12
GitRevision: 998e0e00d375688bcb2af042fc81a60ce5264009
GitBranch: release-0.2
User: releng@0d29a2c0d15f
GolangVersion: go1.8.3
```

## 安装istio核心

简单起见，不做双向认证。方便期间，安装istio-initializer以便自动注入envoy。

```bash
cd /usr/local/share/istio/
kubectl apply -f install/kubernetes/istio.yaml
kubectl apply -f install/kubernetes/istio-initializer.yaml
```

验证安装：

```bash
kubectl get svc -n istio-system
kubectl get pods -n istio-system
```

## 指南实战

参照官方文档指南篇，开始运行其中的例子。

其中bookinf中，有个命令有误，需要增加`-n istio-system`指定namespace。

```bash
 export GATEWAY_URL=$(kubectl get po -l istio=ingress -n istio-system -o 'jsonpath={.items[0].status.hostIP}'):$(kubectl get svc istio-ingress -n istio-system -o 'jsonpath={.spec.ports[0].nodePort}')
```


