---
title:  "kOps Concept"
excerpt: ""

categories:
  - kubernetes
tags:
  - [kubernetes, kops, kubeadm, bootstrap]

toc: true
toc_sticky: true
 
date: 2021-03-23
last_modified_at: 2021-03-23
---

레거시 환경에서는 Kubernetes를 Bootstrap(구축)시 kubeadm이 주로 사용되고, Cloud 환경에서는 'kOps' 나 'kubespray'와 같은 툴들을 사용하여 Kubernestes 환경을 편리하게 구축/관리하게 된다. 그중 kops를 정리한다.

# kOps

{:refdef: style="text-align: center;"}
![CKA](/assets/img/kubernetes/2021-03-23-14-02-18.png)
{: refdef}


'kops'는 kubernetes operation의 약자로 Cloud 환경에 Production-grade의 K8S Cluster를 쉽게 생성/삭제/업그레이드/관리해주는 오픈소스 프로젝트이다. Cluster를 위한 kubectl을 표방하여 kops가 제공하는 Command Line Tool을 사용해 kubernetes cluster를 관리할 수 있다.

## Features

- 완전 자동화된 설치 : 자동으로 Kubernetes를 설치해 준다.
- Self Healing : 자원들을 Auto-Scaling Group을 통해 관리하여 자체 복구가 가능하다.
- 다양한 OS 지원 : Debian, Ubuntu, CentOS, RHEL, Amazon Linux, CoreOS 등을 지원한다.
- HA 지원 : HA 구성으로 kubernetes 환경을 구출 할 수 있다.
- Upgrade 지원 : Kubernetes 업그레이드가 가능하다.
- Terraform 지원 : Terraform 매니페스트를 생성 할 수 있다.

## Cloud Support

2021년 3월 현재 클라우드들에 대한 지원 현황은 아래와 같다.

- 공식 지원 : AWS
- Beta Support : DigitalOcean, GCE, and OpenStack
- Alpha Support : Azure
- 참고
    - AliCloud in deprecated
    - vSphere removed

## Commands

### kops create

cluster를 등록한다.

- spec file로 부터 cluster를 등록 Command:

```bash
kops create -f <cluster spec>
```

- cluster를 등록 Command:

```bash
kops create cluster <cluster name>
```

### kops update

cloud resource들을 실제로 생성하거나 업데이트한다.

- Command:

```bash
kops update cluster <cluster name>
```

### kops rolling-update

kubernetes cluster를 업데이트한다.

- Command:

```bash
kops rolling-update cluster <cluster name>
```

### kops get

cluster들의 정보를 조회한다.

- Command:

```bash
kops get clusters # list
kops get <cluster name> # for a cluster
```

### kops delete

cluster를 등록한다.

- Command:

```bash
kops delete cluster --name
kops delete -f <manifest file>
```

### kops version

kops version을 확인한다.

- Command:

```bash
kops version
```

## AWS Usage

AWS Cloud 환경에 kops을 사용해 kubernetes cluster를 구축하는 과정은 다음과 같다.

### Step1 - AWS IAM 키 생성, 구성, 권한 부여

- IAM 필요 권한:
    - AmazonEC2FullAccess
    - AmazonRoute53FullAccess
    - AmazonS3FullAccess
    - IAMFullAccess
    - AmazonVPCFullAccess

### Step 2 - kops 작업 환경 준비

kops 작업을 위해 Local Host를 사용할 수 있지만 관리적 편의성을 위해 EC2 Instance가 주로 사용된다.

- 필요 작업: kubectl, aws-cli 설치

### Step 3 - kops 설치

실행파일을 다운로드 하거나 소스코드로 부터 빌드한다.

### Step 4 - DNS 설정

kops는 클러스터 내부와 외부 모두에서 검색을 위해 DNS를 사용한다. DNS를 통해 Kubernetes API 서버에 연결한다.

### Step 5 - Create S3 Bucket

kops가 설치 이후에 클러스터를 관리하기 위해 사용자가 생성한 클러스터의 상태나 사용하는 키 정보들을 지속적으로 추적한다. 이러한 정보들을 S3에 저장하여 사용한다.

### Step 6 - Cluster Setting

kops create cluster 혹은 kops edit cluster 를 실행하여 클러스터 설정을 생성/구성한다.

### Step 7 - Create Cluster

kops update cluster 를 실행하여 클러스터 설정으로 부터 환경을 구성한다.

### 최종적으로 생성 된 자원 예시

- EC2 Instances
- Load Balancer
- VPC
- Subnet
- 보안 그룹

---

# Source

- [https://github.com/kubernetes/kops](https://github.com/kubernetes/kops)
- [https://kops.sigs.k8s.io/](https://kops.sigs.k8s.io/)
- [https://kops.sigs.k8s.io/getting_started/commands/](https://kops.sigs.k8s.io/getting_started/commands/)
- [https://kubernetes.io/ko/docs/setup/production-environment/tools/kops/](https://kubernetes.io/ko/docs/setup/production-environment/tools/kops/)