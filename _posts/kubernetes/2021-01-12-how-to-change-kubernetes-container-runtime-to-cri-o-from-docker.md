---
title:  "How to Change Kubernetes Container Runtime To CRI-O from Docker"
excerpt: "About How to change/install Kubernetes CRI-Runtime To CRI-O from Docker"

categories:
  - kubernetes
tags:
  - [kubernetes, cri-runtime, cri-o, crio, docker, cri, runtime]

toc: true
toc_sticky: true
 
date: 2021-01-12
last_modified_at: 2021-01-12
---

# Intro

[[Kubernetes Cluster 구축  Post](https://taesunny.github.io/kubernetes/how-to-install-kubernetes-cluster-with-kubeadm-on-aws-lightsail-instances/)]에서 구성한 Docker Container Runtime의 Kubernetes Cluster 환경에서, Worker Node 하나의 Container Runtime을 CRI-O로 변경하는 작업을 정리한다. 기존 노드 중 'worker2'를 선택한다.

## Cluster Info

- Cluster Node 구성: 3 Lightsail Instances
    - Master Node
    - Worker Node1
    - Worker Node2
- Instance OS: Ubuntu 20.04 LTS
- Kubernetes Version: v1.20.1
- CNI: Flannel
- Container Runtime: Docker v19.3.11

![kubernetes cluster node info](/assets/img/kubernetes/2021-01-12-16-52-42.png)

# Change Kubernetes Container Runtime To CRI-O from Docker

## Install CRI-O On Worker Node

`CRI-O를 설정할 노드(worker2)`에 CRI-O를 설치해 준다. CRI-O의 버전은 Kubernetes와 동일한 버전들이 호환 된다. 아래의 명령어들을 실행하여 CRI-O를 설치해 준다.

> - VERSION 변수를 설치할 Kubernetes 환경의 버전과 동일하게 설정한다.
> - OS 변수를 [[설치 문서 가이드](https://github.com/cri-o/cri-o/blob/master/install.md)]를 확인하여 설정한다.
>     - 예)Ubuntu 20.04: xUbuntu_20.04, Ubuntu 18.04: xUbuntu_18.04

- 아래의 명령들을 쉘에서 root로 실행 하여 설치
    ```bash
    VERSION=1.20
    OS=xUbuntu_20.04

    echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
    echo "deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list

    curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | apt-key add -
    curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | apt-key add -

    apt-get update
    apt-get install cri-o cri-o-runc

    systemctl daemon-reload
    systemctl enable crio
    systemctl start crio
    ```


- 아래의 명령을 통해 running 중인 crio 데몬을 확인 할 수 있다.
    ```
    sudo systemctl status crio
    ```

  ![crio daemon check](/assets/img/kubernetes/2021-01-12-16-57-45.png)

## Drain Worker Node

CRI-O를 설치할 노드(worker2)를 drain하여 Pod들을 evict해 준다.

- Run drain
    ```bash
    kubectl drain {Node Name} --force --ignore-daemonsets
    ```

## Reset kubeadm reset On Worker Node

CRI-O를 설치할 노드(worker2)를 kubeadm reset하여 Cluster에서 제거 해 준다.

> 기존에 kubeadm join시 사용했던 docker의 socket(/var/run/dockershim.sock)을 명시한다.


- Reset the node
    ```bash
    sudo kubeadm reset --cri-socket=/var/run/dockershim.sock
    ```

## Get join command (Create token)

CRI-O Node (worker2)를 다시 join하기 위해 token을 재 발급 받는다. '--print-join-command' 옵션을 사용하여 편리하게 full join command를 얻을 수 있다.

- Create token and Print join command
    ```bash
    sudo kubeadm token create --print-join-command

    // example
    sudo kubeadm join 172.26.15.44:6443 --token jc766n.sqhvprbnwozmbzzz \
      --discovery-token-ca-cert-hash sha256:zzzzfcf98zzzb7zzzf75a728386zzz85a159b3ab54e89db38ac7fce9zzzzzzzz \
      --cri-socket=/var/run/crio/crio.sock
    ```

## Join Worker Node

Join할 CRI-O Node에서 kubeadm join command를 실행해 준다. 앞에서 생성한 token이 포함된 join command에 CRI-O의 cri-socket을 명시해 준다.

> 기존의 docker가 존재 하여 docker의 cri-socket이 남아 있거나, 여러 cri-socket들이 Node에 존재 할 경우, 반드시 join시 사용할 cri-socket을 명시해야 합니다.

- kubeadm join
    ```bash
    // example
    sudo kubeadm join 172.26.15.44:6443 --token jc766n.sqhvprbnwozmbzzz \
      --discovery-token-ca-cert-hash sha256:zzzzfcf98zzzb7zzzf75a728386zzz85a159b3ab54e89db38ac7fce9zzzzzzzz \
      --cri-socket=/var/run/crio/crio.sock
    ```

## Uncordon Worker Node

drain 했었던 Worker Node를 uncordon하여 스케쥴링을 재개 합니다.

- Run uncordon
    ```bash
    kubectl uncordon {node name}
    ```


## Check Worker Node Status and Container Runtime
Container Runtime 교체를 진행한 Node를 Describe하거나, 아래와 같은 방법으로 Node의 상태와 Container Runtime을 확인한다.
```
kubectl get nodes -o wide
```
![container runtime changed node info](/assets/img/kubernetes/2021-01-12-17-32-24.png)

## Pod Creation Test
해당 Node에 Pod을 생성하여, 정상 생성 및 사용이 가능한지 확인한다.

> 해당 Node에 Pod을 스케쥴링 하기 위해, spec.nodeName 필드를 이용하거나, Node Affinity를 사용한다.

- Test Pod Yaml
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: ubuntu-crio-test
    spec:
      containers:
      - image: ubuntu:18.04
        name: testpod
        command: ["/bin/bash", "-ec", "while :; do echo '.'; sleep 5 ; done"]
      nodeName: {Target Node Name}
    ```

- 생성된 Pod 확인
    ![created pod info](/assets/img/kubernetes/2021-01-12-17-34-49.png)

- Pod Container Ping 확인
    ![Container Ping Test](/assets/img/kubernetes/2021-01-12-17-35-12.png)


# Source

- [https://cri-o.io/](https://cri-o.io/)
- [https://github.com/cri-o/cri-o/blob/master/install.md](https://github.com/cri-o/cri-o/blob/master/install.md)