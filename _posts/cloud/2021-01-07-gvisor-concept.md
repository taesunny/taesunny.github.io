---
title:  "gVisor Concept"
excerpt: "gVisor - application kernel"

categories:
  - cloud
tags:
  - [cloud, vm, container, runtime]

toc: true
toc_sticky: true
 
date: 2021-01-07
last_modified_at: 2021-01-07
---

# gVisor

![gVisor](/assets/img/cloud/2021-01-07-23-18-18.png)

기존의 컨테이너는 컨테이너들과 호스트 사이에 커널을 공유하여 VM에 비해 상대적으로 보안에 취약하다는 단점이 있다. 이러한 컨테이너의 단점을 보안하기 위해 gVisor라는 기술이 개발되고 있다.

Apache 2.0 오픈오스이며 Go로 작성된 gVisor는 공식 문서에서 'gVisor is an application kernel for containers', 'a sandboxed container runtime'로 표현된다.

gVisor가 Application(Container)의 System Call Layer 역할을 User Layer에서 수행하여 Application과 Host OS를 분리하는 역할을 한다. 또한, runsc라는 Runtime을 제공하여, gVisor에 의해 Sandboxing된 Container를 제공하는 것이 특징이다.

## Components

![gVisor Components](/assets/img/cloud/2021-01-07-23-18-34.png)

### Sentry
Container를 실행시키는 'Sentry'는 Application(Container)의 System Call을 가로채고 응답하는 Application의 Kenel 역할을 한다. Application에서 필요한 'system calls', 'signal delivery', 'memory management', 'page faulting logic', 'threading model' 등의 Kernel 기능을 구현해 제공한다.

Sentry는 Userpace에서의 Application이기 때문에 모든 Host System Call을 수행하지 못한다. 예를 들어, File들을 직접 Open하지 못하고, sandbox를 넘어서는 File System들에 대에 권한이 없다. 이런 경우 Gofer를 통해 해당 System Call을 수행한다.

### Gofer
Gofer는 Sentry가 권한이 없는 File System 리소스들에 접근할 수 있도록 매개체 역할을 한다. 각 Container들 마다 별도의 'Gofer'가 Host Process로 동작하며, '[9P Protocol](https://en.wikipedia.org/wiki/9P_(protocol))'로 Sentry와, Socket이나 Shared Memory로 통신한다.

### runsc
'runsc'는 'Run Sandboxed Container'의 약자로, gVisor에서 sandboxed container를 제공하기 위해 사용하는 OCI(Open Container Initiative) Runtime이다. OCI Spec을 따르기 때문에 OCI를 따르는 FileSystem을 수행할 수 있다. Docker, Kubernetes를 통해 사용하거나 단독으로 사용할 수 있다.


## 사용 방법
### Docker에서의 gVisor 사용 방법
[gVisor Docker Quick Start](https://gvisor.dev/docs/user_guide/quick_start/docker/)
runsc를 설치 후, v17.09이상의 docker의 daemon.json에 runtime을 명시에 사용할 수 있다.
```
// /etc/docker/daemon.json
{
    "runtimes": {
        "runsc": {
            "path": "/usr/local/bin/runsc"
       }
    }
}
```

```
// run docker
docker run --runtime=runsc --rm -it ubuntu /bin/bash
```

### Kubernetes에서의 gVisor 사용 방법
[gVisor Kubernetes Quick Start](https://gvisor.dev/docs/user_guide/quick_start/kubernetes/)

Minikube, Containerd에서 설정하여 gVisor를 사용하거나 [GKE Sandbox](https://cloud.google.com/kubernetes-engine/docs/concepts/sandbox-pods)에서 gVisor를 이용한 Pod을 사용 할 수 있다.

Minikube에서 사용할 경우 gVisor를 addon으로 쉽게 설치가 가능하고([Minikube gVisor addon page](https://github.com/kubernetes/minikube/blob/master/deploy/addons/gvisor/README.md)), Containerd를 이용한 방법을 이용할 경우에는 runsc를 containerd에서는 사용하는 runtime으로 지정한 후 'containerd-shim-runsc-v1'라는 shim을 이용하는 방법으로 gVisor를 사용하게 된다. ([Containerd gVisor 사용 문서](https://gvisor.dev/docs/user_guide/containerd/quick_start/))

## Compatibility
gVisor에서 아직 모든 system call을 지원하지 않으므로, 2021년 1월 현재 nmap, netstat, tcpdump등의 Application(Container)에서 불가능한 작업들이 있다. 이러한 정식 호환되는 작업 목록은 [정식 가이드](https://gvisor.dev/docs/user_guide/compatibility/)에서 확인 가능하다.



# Source
- https://gvisor.dev/docs/
- https://cloud.google.com/blog/products/gcp/open-sourcing-gvisor-a-sandboxed-container-runtime
- https://gvisor.dev/docs/user_guide/quick_start/docker/