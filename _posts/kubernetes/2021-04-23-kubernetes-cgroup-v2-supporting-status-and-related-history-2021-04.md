---
title:  "Kubernetes Cgroup v2 Supporting Status And Related History (~2021-04)"
excerpt: ""

categories:
  - kubernetes
tags:
  - [kubernetes, cgroup, cgroup-v2, cgroup2]

classes: wide
toc: false
toc_sticky: true
 
date: 2021-04-23
last_modified_at: 2021-04-23
---

2021-04 기준으로 Kubernetes와 관련 Tool, Spec등의 Cgroup v2의 지원 현황을 정리한다.

{:refdef: style="text-align: center;"}
![Kubernetes Cgroup v2 Supporting Status 정리 (2021-04)](/assets/img/kubernetes/2021-04-23-13-53-14.png)
{: refdef}

---

# History

## Cgroup v1 Release (2008-01)

구글에 의해 Cgroup v1이 개발되고, Linux 2.6.24 버전에 포함 되었다.

## Cgroup v2 Release (2016-03)

Linux 4.5에서 Cgroup v2가 공식으로 포함되기 시작 되었다.

### Linux Kernel 정보에 따른 cgroup v2 controllers 추가 정보

- 4.5 : io, memory, pids
- 4.11 : perf_event, rdma
- 4.15 : cpu
- 5.0 : cpuset
- 5.2 : freezer
- 5.6 : hugelb

## Fedora 31 (2019-08-29)

Fedora 31 부터 cgroup v2가 default로 설정 된다.

## cAdvisor Cgroup v2 Supporting Start (2019-09-10)

cAdvisor에서 Cgroup v2 관련 매트릭 모니터링 기능을 지원하기 시작 됨. 아직 cgroup v1에서 지원했던 모든 항목들을 지원하지는 않는다.

[[PR 링크](https://github.com/google/cadvisor/pull/2309)]

> 2021-04 현재 cpu, memory, perf events등 가능

## Kubernetes cgroup v2 Supporting KEP (2020-02-27)

Kubernetes cgroup v2 Supporting KEP(Kubernetes Enhancement Proposal)가 승인 되었다.

[[PR 링크](https://github.com/kubernetes/enhancements/pull/1370)]

## Kubernetes cgroup v2 First Alpha Release (2020-03-27)

K8S v1.19에서 cgroup v2에 대한 첫 alpha release가 포함 되었다.

[[Init Supporting PR 링크](https://github.com/kubernetes/kubernetes/pull/85218)]

[[진행사항 정리 이슈 링크](https://github.com/kubernetes/enhancements/issues/2254)]

## OCI Runtime Spec cgroup v2 Supporting (2020-08-18)

OCI Runtime Spec에 cgroup v2 관련 내용이 추가 되었다.

> 기존 LinuxResources struct에 'Unified'(map[string]string) 필드가 추가 됨

[[PR 링크](https://github.com/opencontainers/runtime-spec/pull/1040)]

[[문서 링크](https://github.com/opencontainers/runtime-spec/blob/master/config-linux.md#unified)]

## runc cgroup v2 Supporting (2021-02-04)

v1.0.0-rc93 버전 부터 cgroup v2를 정식 지원한다.

> 코드를 보면 oci spec의 기존 resource 정보(Memory, CPU)를 cgroup v2 resource 정보로 converting한 후 systemd의 property들로 설정하여 사용한다.

> Kernel 5.2 이후 버전이 권장되며, systemd의 경우 version 244 이후 버전이 권장된다.

[[문서 링크](https://github.com/opencontainers/runc/blob/master/docs/cgroup-v2.md)]

## cri-o cgroup v2 Supporting (2021-02-06)

v1.20부터 pod annotation으로 테스트용 cgroup v2 unified config를 override 하는 기능이 추가되었다. 해당 필드가 설정되어 있을 경우 해당 annotation의 설정들을 OCI Spec-LinuxResouces 객체의 Unified 필드에 설정해 준다.

[[PR 링크](https://github.com/cri-o/cri-o/pull/4479)]

- 예시

  ```YAML
      metadata:
        name: sunny-test
        annotations:
          io.kubernetes.cri-o.UnifiedCgroup.cgroup-test: "memory.high=7000000;memory.low=200000"
  ```

## Docker cgroup v2 Supporting (2021-04-12)

Docker 20.10.6 버전 부터 cgroup v2 정식 지원한다.

[[릴리즈 링크](https://docs.docker.com/engine/release-notes/)]

## 참고) CRI Spec

CRI Spec은 기존과 변화가 없음

[[Spec 링크](https://github.com/kubernetes/cri-api/blob/master/pkg/apis/runtime/v1/api.proto)]

---

# 참고) Linux cgroup v2 활성화 방법

- Fedora 31 이후의 버전이 아닌 경우 Linux Kernel Parameter를 설정하여 cgroup v2를 사용할 수 있다.

    ```
    systemd.unified_cgroup_hierarchy=1
    ```

---

# Source

- https://www.youtube.com/watch?v=u8h0e84HxcE
- 각 Tool, Spec들의 공식 문서
- OCI Spec, runc, docker, cri-o github 프로젝트