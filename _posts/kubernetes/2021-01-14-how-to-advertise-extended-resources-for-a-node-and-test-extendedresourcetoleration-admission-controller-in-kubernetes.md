---
title:  "How to Advertise Extended Resources for a Node And Test ExtendedResourceToleration Admission Controller In Kubernetes"
excerpt: ""

categories:
  - kubernetes
tags:
  - [kubernetes, extendedresourcetoleration, admission-controller]

toc: true
toc_sticky: true
 
date: 2021-01-14
last_modified_at: 2021-01-14
---

# Intro

Kubernetes에서 특정 Node에 Nvidia GPU와 같은 Extended Resource를 추가하고, ExtendedResourceToleration Admission Controller를 사용해 해당 자원을 사용하는 Pod들만 해당 Node에 스케쥴링 되도록 하는 방법을 정리한다.

## 테스트 환경

- kubeadm을 이용해 설치한 Kubernetes v1.19.6 환경
- Ubuntu 18.04

![kubernetes cluster node list](/assets/img/kubernetes/2021-01-14-18-30-56.png)


---
# Advertise Extended Resource for a Node

테스트를 위해 클러스터의 노드 중 하나 - worker2에 테스트용 리소스('sunny.com/gpu')를 추가한다.

## API

> resouce-name을 "sunny.com/gpu" 와 같이 표현하려면 '/'을 '~1'로 변환하여 명시한다. (IETF RFC 6901, section 3 내용)

```
PATCH /api/v1/nodes/<your-node-name>/status HTTP/1.1
Accept: application/json
Content-Type: application/json-patch+json
Host: k8s-master:8080

[
  {
    "op": "add",
    "path": "/status/capacity/<resource-name>", // resource-name example: "sunny.com~1gpu",
    "value": "<number-of-resource>"
  }
]
```

## Run kubectl proxy

kubectl이 가능한 Node에서 kubectl proxy를 동작시켜, API 서버에 localhost를 통해 요청이 가능하도록 한다. 백그라운드로 실행시키거나, kubectl proxy를 실행시킨 후, 다른 터미널에서 작업한다.

> kubectl proxy: localhost 주소로 Kubernetes API 서버로 Proxy해 준다. API 서버를 찾아 인증 헤더를 추가한 후 요청을 Proxy 해 준다.

- 명령어

```bash
kubectl proxy
```

- Port 확인

kubectl proxy 실행 시 출력되는 Port를 이후 proxy 사용 시 설정한다. 아래의 예제와 같이 Port '8001'을 확인 할 수 있다.

![kubectl proxy](/assets/img/kubernetes/2021-01-14-18-32-12.png)

## Request Node Patch API

Proxy를 통해 Node의 객체를 PATCH하여 status.capacity에 리소스를 추가한다.

- 예시 Request

```bash
curl --header "Content-Type: application/json-patch+json" \
--request PATCH \
--data '[{"op": "add", "path": "/status/capacity/sunny.com~1gpu", "value": "7"}]' \
http://localhost:8001/api/v1/nodes/worker2/status
```

- 결과 result

Node 리소스의 PATCH 호출 결과로, status.capacity 필드에 리소스가 추가된 Node 객체가 리턴된다.

![node patch result](/assets/img/kubernetes/2021-01-14-18-32-39.png)

- Node 정보

이후 PATCH를 진행한 Node 정보를 조회 해 보면, status.allocatable 필드에도 리소스가 추가 된 것을 확인 할 수 있다.

![node describe result](/assets/img/kubernetes/2021-01-14-18-32-59.png)

## 참고) How to Clean Up Advertised Extended Resource

PATCH의 'remove' 기능을 사용해 추가한 리소스를 제거 할 수 있다.

- API

```
PATCH /api/v1/nodes/<your-node-name>/status HTTP/1.1
Accept: application/json
Content-Type: application/json-patch+json
Host: k8s-master:8080

[
  {
    "op": "remove",
    "path": "/status/capacity/<resource-name>" // resource-name example: "sunny.com~1gpu",
  }
]
```

- 예시 Request

```bash
curl --header "Content-Type: application/json-patch+json" \
--request PATCH \
--data '[{"op": "remove", "path": "/status/capacity/sunny.com~1gpu""}]' \
http://localhost:8001/api/v1/nodes/worker2/status
```

---
# About ExtendedResourceToleration Admission Controller

ExtendedResourceToleration Admission Controller를 활성화 하면, Pod에 Extended Resource를 사용할 경우 자동으로 Pod에 해당 Resource 이름을 Key로 하는 Toleration을 추가 해 준다. Node에 Resource 이름으로 Taint를 추가하여, 해당 Resource를 사용하는 Pod들만 스케쥴링 되도록 활용 할 수 있다.

> kube-apiserver에 기본으로 활성화 되는 Admission Controller가 아님

---
# Enable ExtendedResourceToleration Admission Controller In kube-proxy

kubeadm으로 Kubernetes Cluster를 구축한 경우 Master Node에서 Static Pod으로 kube-apiserver Pod이 생성 된다. 'ExtendedResourceToleration' Admission Controller는 kube-apiserver에서 기본적으로 활성화된 Admission Controller가 아니므로, 추가로 Enable해 준다. 이를 위해 kube-apiserver의 Static Pod 명세를 수정한다.

> Static Pod들의 경우 기본적으로 '/etc/kubernetes/manifests/' 디렉토리에 Static Pod 명세 yaml 파일로서 설정된다.

- /etc/kubernetes/manifests/kube-apiserver.yaml 파일 수정 후 저장
spec.containers[].command의 —enable-admission-plugins 인자에 'ExtendedResourceToleration'를 추가하여, ExtendedResourceToleration Admission Controller를 활성화한다. 파일 수정 후 저장 시, kube-apiserver Pod이 자동으로 재 생성 되는 것을 확인 할 수 있다.

```yaml
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: 10.0.0.2:6443
  creationTimestamp: null
  labels:
    component: kube-apiserver
    tier: control-plane
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
  - command:
    ...
    - --enable-admission-plugins=NodeRestriction,ExtendedResourceToleration
    ...
```

---
# Pod Toleration Test

ExtendedResourceToleration Admission Controller의 동작을 확인해본다.

자원을 추가한 Node에 자원 이름으로 Taint를 추가 한후 해당 자원을 사용하는 Pod을 생성하여, Pod에 해당 Toleration이 자동으로 추가 되어 스케쥴링이 가능한지를 확인한다.

## Add Taint to the Node

자원을 추가한 Node에 자원이름으로 Taint를 추가한다.

- 아래의 명령을 실행한다.

```bash
# kubectl taint node <node-name> <resource-name>=:NoSchedule
kubectl taint node worker2 sunny.com/gpu=:NoSchedule
```

- Node 객체 조회 시 Taints 항목에 Taint가 추가 되었다.

![node taints result](/assets/img/kubernetes/2021-01-14-18-34-37.png)

## Create A Pod

앞에서 추가한 자원을 사용하도록 Pod의 resource 항목에 자원을 추가하여 Pod을 생성한다.

- Pod Yaml 예시

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: ubuntu
spec:
  containers:
    - name: ubuntu
      image: ubuntu:18.04
      resources:
        requests:
          sunny.com/gpu: 3
        limits:
          sunny.com/gpu: 3
      command: ["/bin/bash", "-ec", "while :; do echo '.'; sleep 5 ; done"]
  restartPolicy: Never
```

- ExtendedResourceToleration Admission Controller에 의해 Pod에 자동으로 자원 이름('sunny.com/gpu') 을 key로 하는 Toleration이 추가 되어, 해당 Node에 정상 생성 된 것을 알 수 있다.

![created pod result](/assets/img/kubernetes/2021-01-14-18-35-06.png)

![created pod's tolerations](/assets/img/kubernetes/2021-01-14-18-35-22.png)

## 주의할 점

Node에 'NoSchedule' operator의 Taint를 이용하는 방법으로는, Pod Spec에 `nodeName`과 같이 스케쥴러를 거치지 않는 Pod의 스케쥴링을 방지 할 수 없다.

완벽하게 스케쥴링을 방지하기 위해서는 Node에 Taint를 'NoExecute' operator로 추가 후, 직접 구현한 Admission Controller 등을 사용하여 spec.nodeName 필드와 Toleration 필드를 Mutation & Validation 해야한다.

혹은 자원이 존재 하는 Node 또는 자원이 존재 하지 않는 Node들에 Label을 추가 한 후, Admission Controller를 이용해 Pod들에 Node Affinity를 자동으로 Mutation & Validation 한다.

---
# Source

- [https://kubernetes.io/docs/tasks/administer-cluster/extended-resource-node/](https://kubernetes.io/docs/tasks/administer-cluster/extended-resource-node/)
- [https://kubernetes.io/ko/docs/concepts/cluster-administration/proxies/](https://kubernetes.io/ko/docs/concepts/cluster-administration/proxies/)
- [https://kubernetes.io/ko/docs/concepts/scheduling-eviction/taint-and-toleration/](https://kubernetes.io/ko/docs/concepts/scheduling-eviction/taint-and-toleration/)
- [https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#extendedresourcetoleration](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#extendedresourcetoleration)