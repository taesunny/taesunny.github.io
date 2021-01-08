---
title:  "Kata Containers Concept"
excerpt: "Kata Containers is an open source container runtime using lightweight VM"

categories:
  - cloud
tags:
  - [cloud, vm, container, runtime, katacontainers]

toc: true
toc_sticky: true
 
date: 2021-01-08
last_modified_at: 2021-01-08
---

# Kata Containers

![Kata Containers Logo](/assets/img/cloud/2021-01-08-17-11-54.png)

Kata Container는 경량 VM을 사용하여 보안이 강화된 Container를 제공하는 Apache 2.0의 Open Source Container Runtime이다.

2017년 12월 OpenStack에 의해 시작되었으며, 경량화된 VM에 Container를 띄우기 위한Intel의 Clear Containers와 VM을 Container처러 만들기 위한 기술인 Hyper.sh의 runV 프로젝트를 통합해 Kata Container가 최초 릴리즈되었다.


## Components
![Kata Containers Structure](/assets/img/cloud/2021-01-08-17-15-54.png)

### VM/Kernel
Linux System을 제공하며, 빠른 부팅과 메모리 사용량이 적은 최적화된 커널을 사용한다.

### kata-agent
VM 내부에서 systemd에 의해 kata-agent가 실행된다. Container, Network, Memory, Cgroup 등을 관리하며, libcontainer(runc와 대부분의 코드를 공유하는 runtime)를 사용한다.

### kata-runtime
Docker, Kubernetes 등에 OCI Spec을 제공하며, Container의 상태를 관리한다.

보통 Container Process를 생성/종료 및 관리를 하거나 모니터링 할 때는, 'containerd-shim'(for Docker or containerd) 또는 'conmon'(for CRI-O)가 이러한 역할을 하게 된다. Kata Containers의 경우에는 VM 내부에 Container Process가 실행 되기 때문에, 이 Process를 관리 할 수 없는 문제가 있다. 이를 해결하기 위해 kata-runtime이 Container별로 'kata-shim' 데몬을 띄워서 OCI 명령어들을 VM 내부의 Container에 실행한다.

> - containerd-shim 역할 예시
>     - runc를 이용해 container process를 생성
>     - Std IO, fd 유지
>     - Exit Code 확인, 모니터링
>     - Container Process의 부모 역할
>     - containerd와 의존성 제거

### kata-shim
Docker, Kubernetes 등의 Container Orchestration들에게 I/O(stdin/stdout/stderr handling, Signal Handling 등)기능을 제공하며, VM 외부에 필요한 정보를 전달한다.

### kata-proxy
virtio-serial을 사용해 kata-shim과 kata-runtime이 VM 내부의 kata-agent와 통신할 수 있도록 proxy 기능을 제공한다.

### Kata Shim V2
![Kata Shim V2](/assets/img/cloud/2021-01-08-17-25-56.png)

최근 kata containers의 kata-shim, kata-runtime, kata-proxy들이 'Kata Shim V2'로 통합되었다.

[Containerd Runtime V2-Shim API](https://github.com/containerd/containerd/tree/master/runtime/v2)를 지원하여 containerd에서 이 Shim API를 사용 가능하다.

![Containerd Runtime V2-Shim structure](/assets/img/cloud/2021-01-08-19-56-23.png)

## VMM (Virtual Machine Manager) Setting
Kata Container에서 VM Backend는 Pluggable하게 설정이 가능하다. 이러한 점 덕분에 'firecracker', 'QEMU', 'QEMU-lite', 'NEMU' 등을 VMM으로 설정이 가능하다.

예를 들어 Extensive한 HW support가 필요할 경우에 QEMU를 선택할 수 있고, 적은 Memory를 사용하기 위해서나 Micro Service들을 위해서는 firecracker를 선택해 사용할 수 있다.

## Shared File System Support
Shared File System으로 File Level로 동작하는 'virtio-9p'와 Block Level로 동작하는 DeviceMapper(virtio-blk or virtio-scsi)를 지원한다.

'virtio-9p'의 경우에는 성능과 기능상 이슈가 있어 Production에서 사용하기 위해서는 DeviceMapper 방식을 사용해야한다. DeviceMapper 방식은 더 높은 성능을 제공하지만 docker의 최신 버전과 호환이 불가하다. 예) docker cp를 통해 Container File in/out 불가

Redhat에서는 'virtio-9p'를 대체하기 위해 POSIX를 완전히 만족하는 'virtio-fs'를 개발했다.

## Networking Support
Podman과 Kubernetes를 위한 CNI와 Docker를 위한 CNM를 호환하며, 각각의 VM들을 위해 Network Namespace들을 생성한다.

Host Network를 재사용 할 수 없다는 한계가 있다.

## With Docker and Kubernetes
![Docker and Kata Container](/assets/img/cloud/2021-01-08-19-22-43.png)

Kata Containers는 OCI Spec을 호환해서, CRI-O, containerd에서 사용하는 Container Runtime으로 설정해 사용이 가능하다. 이러한 방식으로 Docker에서 사용하는 containerd에서 kata containers를 사용하도록 하여, Docker에서도 사용 할 수 있으며, Kubernetes에서도 CRI-O 또는 containerd에 Kata Container runtime을 사용하도록 설정하여 Kata Container가 실행되도록 할 수 있다.

Kubernetes에서는 RuntimeClass라는 기능도 지원하여, 다양한 CRI 종류들과 CRI 설정들을 선택하여 Pod을 생성할 수 있다.


# Source
- https://katacontainers.io/
- https://mkr.acrofan.com/article_sub3.php?number=126637
- https://www.suse.com/media/presentation/TUT1201_Bringing_Container_Security_to_the_Next_Level_Using_Kata_Containers.pdf
- https://www.openstack.org/news/view/382/kata-containers-project-releases-10-to-build-secure-container-infrastructure
- https://www.zdnet.com/article/kata-containers-rewritten-in-rust-and-gets-a-major-speed-boost/
- https://kubernetes.io/ko/docs/concepts/containers/runtime-class/
- https://docs.starlingx.io/operations/kata_container.html
- https://github.com/kata-containers/documentation/blob/master/how-to/containerd-kata.md
- https://github.com/kata-containers/documentation/blob/master/design/architecture.md
