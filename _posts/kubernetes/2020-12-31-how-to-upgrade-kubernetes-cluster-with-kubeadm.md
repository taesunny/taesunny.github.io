---
title:  "How to Upgrade Kubernetes Cluster with kubeadm"
excerpt: "About Kubernetes Upgrade with kubeadm."

categories:
  - kubernetes
tags:
  - [kubernetes, upgrade, kubeadm]

toc: true
toc_sticky: true
 
date: 2020-12-31
last_modified_at: 2020-12-31
---

Kubernetes에서 kubeadm으로 구성된 환경의 버전을 upgrade하는 방법을 정리한다.

예제는 Ubuntu 18.04, Kubernetes v1.16.3 환경에서, Kubernetes v1.17으로 Upgrade하는 상황을 기준으로 테스트를 진행한다.

- 참고) 기존 환경 정보

    ![v1.16.3 k8s node version info](/assets/img/kubernetes/2020-12-31-14-27-45.png)

# Kubernetes Version

kubernetes version은 아래와 같이 `Major version`, `Minor version`, `Patch Version` 으로 나뉜다.

![kubernetes version info](/assets/img/kubernetes/2020-12-31-14-29-06.png)

kubeadm을 이용한 Cluster Upgrade에서는 여러개의 minor version을 한번에 upgrade하는 기능은 제공되지 않는다. 따라서, `한번에 하나의 버전을 Upgrade` 한다. Kubernetes 버전 및 OS 별로 Upgrade하는 방법은 [[문서](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)] 또는 아래의 링크들에서 확인할 수 있다.

> - [Upgrading a kubeadm cluster from 1.18 to 1.19](https://v1-19.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)
> - [Upgrading a kubeadm cluster from 1.17 to 1.18](https://v1-18.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)
> - [Upgrading a kubeadm cluster from 1.16 to 1.17](https://v1-17.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)
> - [Upgrading a kubeadm cluster from 1.15 to 1.16](https://v1-16.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

# Upgrade Kubernetes

Cluster를 Upgrade할 때에는 Control plane node(Master Node)들을 먼저 Upgrade 한 후, 나머지 Worker node들을 Upgrade한다.

## Step 1 - Upgrade할 버전을 확인한다.

apt-cache madison 명령으로 가능한 kubeadm 버전을 확인 할 수 있다.

```bash
apt update
apt-cache madison kubeadm
# find the latest 1.17 version in the list
# it should look like 1.17.x-00, where x is the latest patch
```
![apt-cache madison kubeadm example](/assets/img/kubernetes/2020-12-31-14-30-43.png)

## Step 2 - Upgrading Control Plane Nodes

### Upgrade kubeadm

kubeadm을 upgrade한다.

- apt-mark hold/unhold : 패키지가 자동으로 설치, 업데이트 되는 것을 방지/해제 한다. 필요한 경우 실행한다.

```bash
# replace x in 1.17.x-00 with the latest patch version
apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.17.x-00 && \
apt-mark hold kubeadm

# since apt-get version 1.1 you can also use the following method
apt-get update && \
apt-get install -y --allow-change-held-packages kubeadm=1.17.x-00
```

### Drain the control plane node

control plane node(master node)를 drain한다.

```bash
# replace <node-to-drain> with the name of your node you are draining
kubectl drain <node-to-drain> --ignore-daemonsets
```

![kubectl node drain master node example](/assets/img/kubernetes/2020-12-31-14-31-57.png)

### Check the upgrade plan with kubeadm

kubeadm의 upgrade plan 명령어로, 컴포넌트 별 가능한 upgrade 정보를 확인 할 수 있다.

```bash
kubeadm upgrade plan
```

![kubeadm upgrade plan example](/assets/img/kubernetes/2020-12-31-14-32-48.png)

### Upgrade apply

upgrade apply를 통해 원하는 버전으로 upgrade한다. 아래의 실행 결과와 같이, upgrade 완료 후 SUCCESS 출력을 확인할 수 있다.

```bash
# replace x with the patch version you picked for this upgrade
sudo kubeadm upgrade apply v1.17.x
```

![kubeadm upgrade apply example1](/assets/img/kubernetes/2020-12-31-14-33-16.png)
... 생략
![kubeadm upgrade apply example2](/assets/img/kubernetes/2020-12-31-14-34-02.png)

### Upgrade CNI manually

CNI plugin의 upgrade를 원할 경우, 본인의 CNI 별 upgrade 방식에 따라 수동으로 upgrade해 준다.

### Uncordon the control plane node

node를 uncordon하여 스케쥴링을 재개한다.

```bash
# replace <cp-node-name> with the name of your control plane node
kubectl uncordon <cp-node-name>
```
![kubectl uncordon example](/assets/img/kubernetes/2020-12-31-14-34-23.png)

### Upgrade additional control plane nodes

만약 추가 control plane node가 있다면 아래의 명령을 통해 node를 upgrade해 준다.

```bash
sudo kubeadm upgrade node
```

### Upgrade kubelet and kubectl

kubeadm을 통해 upgrade되지 않는 kubelet과 kubectl을 upgrade한다.

```bash
# replace x in 1.17.x-00 with the latest patch version
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.17.x-00 kubectl=1.17.x-00 && \
apt-mark hold kubelet kubectl

# since apt-get version 1.1 you can also use the following method
apt-get update && \
apt-get install -y --allow-change-held-packages kubelet=1.17.x-00 kubectl=1.17.x-00
```

### Restart kubelet

kubelet을 restart 하여 새로운 버전의 kubelet을 실행한다.

```bash
sudo systemctl restart kubelet
```

## Step 2 - Upgrading worker nodes

control plane node와 같이 worker node들에 접속해 upgrade해 준다.

### Upgrade kubeadm

kubeadm을 upgrade한다.

- apt-mark hold/unhold : 패키지가 자동으로 설치, 업데이트 되는 것을 방지/해제 한다. 필요한 경우 실행한다.

```bash
# replace x in 1.17.x-00 with the latest patch version
apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.17.x-00 && \
apt-mark hold kubeadm

# since apt-get version 1.1 you can also use the following method
apt-get update && \
apt-get install -y --allow-change-held-packages kubeadm=1.17.x-00
```

### Drain the worker node

worker node를 drain한다. kubectl 이 가능한 node에서 실행한다.

```bash
# replace <node-to-drain> with the name of your node you are draining
kubectl drain <node-to-drain> --ignore-daemonsets
```
![kubectl drain worker node example](/assets/img/kubernetes/2020-12-31-14-35-07.png)

### Upgrade the kubelet configuration

`kubeadm upgrade node` 명령을 통해 cluster의 설정을 적용하고, Static Pod 정보 upgrade 및 kubelet의 configuration을 upgrade한다.

```bash
sudo kubeadm upgrade node
```

![kubeadm upgrade node example](/assets/img/kubernetes/2020-12-31-14-35-58.png)

### Upgrade kubelet and kubectl
kubeadm을 통해 upgrade되지 않는 kubelet과 kubectl을 upgrade한다.

```bash
# replace x in 1.17.x-00 with the latest patch version
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.17.x-00 kubectl=1.17.x-00 && \
apt-mark hold kubelet kubectl

# since apt-get version 1.1 you can also use the following method
apt-get update && \
apt-get install -y --allow-change-held-packages kubelet=1.17.x-00 kubectl=1.17.x-00
```

### Restart kubelet

kubelet을 restart 하여 새로운 버전의 kubelet을 실행한다.

```bash
sudo systemctl restart kubelet
```

### Uncordon the control plane node

node를 uncordon하여 스케쥴링을 재개한다.

```bash
# replace <cp-node-name> with the name of your control plane node
kubectl uncordon <cp-node-name>
```

## Verify the status of the cluster

kubectl의 node 정보를 통해 upgrade된 node의 버전 (kubelet 버전)을 확인할 수 있다.

```bash
kubectl get nodes
```

![kubectl upgraded node version info example](/assets/img/kubernetes/2020-12-31-14-36-40.png)

또한, System Pod의 Static Pod 명세나, upgrade된 시스템 Pod들의 image 정보를 추가적으로 확인 가능하다.

![upgraded kube-apiserver image example](/assets/img/kubernetes/2020-12-31-14-37-34.png)

# Source

- [https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)
- [https://github.com/kubernetes/community/blob/master/contributors/design-proposals/release/versioning.md#kubernetes-release-versioning](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/release/versioning.md#kubernetes-release-versioning)
- [https://kubernetes.io/docs/setup/release/version-skew-policy/](https://kubernetes.io/docs/setup/release/version-skew-policy/)
- [https://wnw1005.tistory.com/363](https://wnw1005.tistory.com/363)