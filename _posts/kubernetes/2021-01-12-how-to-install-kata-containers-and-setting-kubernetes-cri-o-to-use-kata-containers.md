---
title:  "How to Install Kata Containers and Setting Kubernetes CRI-O to Use Kata Containers"
excerpt: "About How to Install Kata Containers and Setting Kubernetes CRI-O to Use Kata Containers"

categories:
  - kubernetes
tags:
  - [kubernetes, cri-runtime, cri-o, crio, kata, katacontainers, kata-containers, install, cri, runtime]

toc: true
toc_sticky: true
 
date: 2021-01-12
last_modified_at: 2021-01-12
---

# Intro

Kata Containers를 설치하고 Kubernetes 환경의 CRI-O에 설정해서 사용 하는 방법을 정리한다.

## 테스트 환경

- CentOS: 7
- Kernel: 3.10.0
- Kubernetes Version: v1.17.6
- Kubernetes Container Runtime: CRI-O

## 사전 지식
- [Kata Containers 개념 정리 문서](https://taesunny.github.io/cloud/katacontainers-concept/)
- Pod 생성 시, CRI-O의 Kata Containers Runtime 설정을 위해 Kubernetes RuntimeClass를 사용한다. [RuntimeClass 문서](https://kubernetes.io/ko/docs/concepts/containers/runtime-class/)

---
# Install Kata Containers

Kata Containers를 사용할 Kubernetes의 Worker Node에 접속해 Kata Containers를 설치해 준다.

## Install Kata Containers pacakges

[[설치 문서](https://github.com/kata-containers/documentation/blob/master/install/centos-installation-guide.md)]를 참고해 관련 패키지들을 설치해 준다.

> VERSION 변수를 [[설치 문서 링크](https://github.com/kata-containers/documentation/blob/master/install/centos-installation-guide.md)]를 확인해 설정한다. ex) CentOS 7 인 경우 VERSION=7

> yum-config-manager —add-repo 명령으로 repo 추가 시, 설치 문서 가이드를 이용해 repo url 구성이 힘들 경우 [[레포지터리 사이트](http://download.opensuse.org/repositories/home:/katacontainers:/releases:/x86_64:/)]에서 url을 확인 할 수 있다.
> 
> ex) CentOS7, v1.11: http://download.opensuse.org/repositories/home:/katacontainers:/releases:/x86_64:/stable-1.11/CentOS_7/home:katacontainers:releases:x86_64:stable-1.11.repo

```bash
$ source /etc/os-release
$ sudo yum -y install yum-utils
$ ARCH=$(arch)
$ BRANCH="${BRANCH:-master}"
$ VERSION="{your version}"
$ sudo -E yum-config-manager --add-repo "http://download.opensuse.org/repositories/home:/katacontainers:/releases:/${ARCH}:/${BRANCH}/CentOS_${VERSION_ID}/home:katacontainers:releases:${ARCH}:${BRANCH}.repo"
$ sudo -E yum -y install kata-runtime kata-proxy kata-shim
```

- 패키지 설치 확인

    kata-proxy, kata-runtime, kata-shim 패키지와 의존성 패키지들이 설치 된 것을 확인 할 수 있다.

    > 기본으로 qemu-vanilla 버전이 설치된다. 필요에 따라 다른 VMM들을 설치 및 설정할 수 있다.

    ![kata containers installed packages](/assets/img/kubernetes/2021-01-12-21-01-28.png)

## Kata Containers Host Check

kata-runtime의 kata-check 명령으로 Kata Containers를 사용할 수 있는지를 테스트 할 수 있다.

```bash
sudo kata-runtime kata-check
```

- 결과 예시

    ![kata-runtime kata-check](/assets/img/kubernetes/2021-01-12-21-01-58.png)

---
# Setting Kubernetes CRI-O to use Kata Containers

Kubernetes Cluster에서 Kata Container를 사용해 Pod 생성이 가능하도록 설정한다. 테스트할 Node의 CRI-O에 Kata Containers를 설정한다.

## CRI-O Runtime & Networking Setting

CRI-O의 설정파일 '/etc/crio/crio.conf'에 kata containers를 위한 설정을 추가한다.

- /etc/crio/crio.conf 파일 수정

```bash
sudo vi /etc/crio/crio.conf
```

- /etc/crio/crio.conf 파일의 [cri.runtime] 테이블 이후에 다음의 테이블 설정을 추가한다.
    > runtime_path를 kata-runtime의 바이너리를 가르키도록 설정한다.

    > [crio.runtime.runtimes.{Handler Name}] 형식으로, 이후 Kubernetes에서 사용할 Handler Name을 설정해야한다.

```yaml
# append this content into the file
[crio.runtime.runtimes.kata-qemu]
  runtime_path = "/usr/bin/kata-runtime"
  runtime_type = "oci"
```

- /etc/crio/crio.conf 파일의 [cri.runtime] 테이블에 'manage_ns_lifecycle' 필드 설정을 추가한다.  (for CRI-O v1.17+)
    > 설치 문서 발췌 : manage_ns_lifecycle=true Determines whether we pin and remove namespaces and manage their lifecycle. This option is being deprecated, and will be unconditionally true in the future. (In CRI-O v1.16의 경우 : manage_network_ns_lifecycle = true 를 설정)

```yaml
[crio.runtime]
...
manage_ns_lifecycle = true
```

- CRI-O 데몬을 재시작 해 준다.

```bash
sudo systemctl restart crio
```

## Create Kata Container RuntimeClass

CRI-O Config에 설정한 crio.runtime.runtimes 설정의 Handler Name을 이용해 RuntimeClass를 생성해 준다.

> Kubernetes v1.20+ 버전의 경우 GA 기능으로 변경되어, 'apiVersion: node.k8s.io/v1'의 api version을 사용한다.

```yaml
kind: RuntimeClass
apiVersion: node.k8s.io/v1beta1
metadata:
    name: kata-qemu
handler: kata-qemu # crio runtimes handler name
```

## Create Pod using Kata Containers RuntimeClass

생성할 Pod 명세의 spec.runtimeClassName 필드에 앞에서 생성한 RuntimeClass의 이름을 설정한다. 또한 테스트를 위해, spec.nodeName 필드를 사용해 Kata Containers를 설치한 Node로의 스케쥴링 설정을 추가한다.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: kata-ubuntu
spec:
  containers:
    - name: kata-ubuntu
      image: ubuntu:18.04
      command: ["/bin/bash", "-ec", "while :; do echo '.'; sleep 5 ; done"]
  restartPolicy: Never
  nodeName: {your target node name}
  runtimeClassName: kata-qemu
```

## Kata Containers Verification

Kata Containers를 이용해 생성된 Pod을 검증해 본다.

### Running Pod 확인
kubectl을 사용해 생성 완료된 Pod을 확인한다.

![check running pod](/assets/img/kubernetes/2021-01-12-21-03-26.png)

### Ping 확인

앞에서 확인한 Pod의 IP로 Ping을 확인해 본다.

![check pod ping](/assets/img/kubernetes/2021-01-12-21-04-08.png)

### Container Kernel Check

Container에 exec을 통해 접속해 Kernel을 확인 후, Worker Node Host의 Kernel과 비교해 본다.

- Container 내부의 Kernel
    ![kernel inside Container](/assets/img/kubernetes/2021-01-12-21-04-45.png)

- Worker Node의 Host Kernel
    ![host kernel](/assets/img/kubernetes/2021-01-12-21-05-34.png)

### kata-runtime List Check

Pod이 생성된 Worker Node에서 다음의 명령을 통해 kata-runtime을 통해 생성된 Container 목록을 확인 할 수 있다.

```bash
kata-runtime list
```

![kata-runtime list](/assets/img/kubernetes/2021-01-12-21-06-07.png)

### kata-runtime State Check

Pod이 생성된 Worker Node에서 다음의 명령을 통해 kata-runtime을 통해 생성된 Container들의 상태를 확인 할 수 있다. 'pod_sandbox' 타입의 컨테이너와 'pod_container' 타입의 container type을 확인할 수 있다.

> 'pod_sandbox' 타입 : Pod의 Sandboxing을 위한 Container(Pause Container)로, 네임스페이스 분리와 유지를 위한 컨테이너가 Pod에 필요하다.

```bash
kata-runtime state {Container ID}
```

![kata-runtime state](/assets/img/kubernetes/2021-01-12-21-06-33.png)

### QEMU Process Check

Pod이 생성된 Worker Node에서 다음의 명령을 통해 kata-container의 qemu process를 확인 할 수 있다. 'qemu-vanilla-sy' 프로세스를 확인 가능하다.

```bash
ps -ef | grep -E "qemu"
```

![kata containers qemu process](/assets/img/kubernetes/2021-01-12-21-07-01.png)


---
# Source

- [https://github.com/kata-containers/documentation/blob/master/Developer-Guide.md](https://github.com/kata-containers/documentation/blob/master/Developer-Guide.md)
- [https://github.com/kata-containers/documentation/blob/master/how-to/run-kata-with-k8s.md](https://github.com/kata-containers/documentation/blob/master/how-to/run-kata-with-k8s.md)
- [https://github.com/cri-o/cri-o/blob/master/docs/crio.conf.5.md](https://github.com/cri-o/cri-o/blob/master/docs/crio.conf.5.md)
- [https://github.com/kata-containers/documentation/blob/master/install/centos-installation-guide.md](https://github.com/kata-containers/documentation/blob/master/install/centos-installation-guide.md)