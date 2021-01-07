---
title:  "Container Instance Services & Kubernetes Services of Public Cloud Providers (AWS, GCP, Azure, TOAST, NaverCloud)"
excerpt: "Public Cloud Provider들의 Container Instance, Kubernetes 서비스 분석"

categories:
  - cloud
tags:
  - [cloud, kubernetes, aws, gcp, azure, toast, navercloud]

toc: true
toc_sticky: true
 
date: 2021-01-06
last_modified_at: 2021-01-06
---

컨테이너와 Kubernetes Cluster의 관리적 측면에서 퍼블릭 클라우드 프로바이더들의 컨테이너와 쿠버네티스를 제공하는 모델들에 대해 정리한다. 공식 홈페이지와 문서를 기준으로 정리한다.

> 설명이 부족하거나, 잘못된 부분이 있을 수 있습니다. 각 제품의 공식 문서들을 확인 부탁드립니다.

# AWS (Amazon Web Services)
![AWS](/assets/img/analysis/2021-01-06-19-52-03.png)

AWS는 ECS라는 Container 오케스트레이션 서비스와 Kubernetes를 제공하는 EKS 서비스를 제공한다.

## Amazon ECS (Amazon Elastic Container Service)
![AWS ecs flow structure](/assets/img/analysis/2021-01-06-20-00-12.png)

AWS에서 제공하는 '완전관리형 컨테이너 오케스트레이션 서비스'이다. 기본적으로 'EC2 Cluster'를 사용할 수 있고, 'AWS Fargate'를 사용하는 서버리스 옵션을 제공하는 것이 특징이다. 서버리스 옵션을 사용할 경우 서버를 프로비저닝 하거나 관리할 필요가 없고, 컨테이너 앱별로 리소스를 지정하고 비용을 지불하면 된다. 컨테이너 앱별로 격리되기 때문에 보안적으로 안전하다.

> AWS Fargate : 컨테이너를 위한 서버리스 컴퓨팅 엔진, ECS 또는 EKS에서 사용.

EC2 Instance를 사용할 경우 VM들에 Container Agent가 실행되어 ECS의 Container Task들을 수행하고 관리하며, Fargate를 이용할 경우 내부적으로 'Firecracker'를 이용하게 된다.

> Firecracker : Linux KVM을 기반으로 하는 가상 머신 매니저(VMM)를 사용해 microVM(경량 가상머신)를 REST 서비스로 제공한다. microVM은 최소한의 디바이스 모델을 제공하며 빠른 시작 시간이 특징이다. [Firecracker 정리](https://taesunny.github.io/analysis/firecracker/)

## Amazon EKS (Amazon Elastic Kubernetes Service)
![AWS eks flow structure](/assets/img/analysis/2021-01-06-20-11-47.png)

ECS와 함께 AWS에서 제공하는 관리형 Kubernetes 서비스이다. 클라우드 혹은 온프레미스에서 Kubernetes 환경을 제공하는 것이 특징이다.

Control Plane Node들을 EKS에서 관리하며, 기본적으로 3개의 Availability Zone들에서 Control Plane을 구성하는 방식으로 HA를 제공한다. 비정상 Control Plane Node를 자동으로 감지하고 교체하며 Kubernetes API 서버를 ELB로 제공하여 가용성을 제공한다.

EKS Worker Node Cluster의 Computing 옵션은 세가지로 분류된다.

1. Fargate
2. 관리형 Node
3. 자체 관리형 Node

'Fargate' 옵션을 사용할 경우 Linux 기반의 Container를 사용할 수 있다.

'관리형 Node' 옵션을 사용할 경우 AWS가 '노드(Linux EC2 Instance)들의 그룹' 프로비저닝 및 수명 주기를 자동으로 관리해 주며, 노드 그룹의 상태 문제가 발생하면 EKS가 오류 메시지를 반환한다. Instance들이 Auto Scaling 그룹의 일부로 프로비저닝 되고, 사용자는 자동으로 생성된 라벨들을 이용해서 Kubernetes Cluster AutoScaler를 사용할 수 있다.

> [Kubernetes Cluster AutoScaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler
): Kubernetes가 제공하는 'Cluster 크기를 자동으로 조절해 주는 Tool'. Pod을 생성할 자원이 부족하거나 Node의 사용률이 낮을 때 Node들의 수가 자동으로 조절된다. Cloud Provider들의 Support가 필요하다.

'자체 관리형 Node' 옵션을 사용할 경우 '노드 그룹'을 사용자가 생성하여 Kubernetes Cluster에 추가하게 된다. 'Windows' 인스턴스 그룹도 사용할 수 있다는 것이 특징이다.
> ‘자체 관리형 Node’ 옵션의 기타 특징
> - 클러스터에 Linux Node 하나이상 필요
> - 각 노드 그룹은 동일한 인스턴스 유형, AMI, 노드 IAM을 사용해야함


# Google Cloud Platform
![Google Cloud Logo](/assets/img/analysis/2021-01-06-19-20-43.png)

## Google Kubernetes Engine (GKE)

GCP에서도 '자동 확장과 멀티 클러스터 지원을 갖춘 안전한 관리형 Kubernetes 서비스'를 제공하고 있다.

![GKE Cluster Architecture](/assets/img/analysis/2021-01-06-19-28-46.png)

Google의 Compute Engine VM을 이용해 Node들을 구성하며, VPC, Persistent Disk, LB등의 Google Cloud의 다른 서비스들과 연동할 수 있다.

Control Plane은 GKE가 관리하고, Node들을 사용자가 관리하며, 'Node 자동 복구 기능'을 제공한다는 점이 특징이다.

### Node 자동 복구 기능

직접 설치하거나 COS(Container-Optimizied OS)에 자동 설치된 [Kubernetes 노드 문제 감지기](https://github.com/kubernetes/node-problem-detector) 에이전트를 이용해 인스턴스의 상태를 모니터링 할 수 있다. 노드 풀 단위로 해당 기능을 설정하며, GKS가 노드 문제 감지기의 모니터링 정보를 바탕으로 복구 프로세스를 수행한다. 복구 프로세스 동작 시 해당 노드가 Drain 되고 다시 생성된다.

복구 기준은 다음과 같다.
- 일정 시간 임계값(약 10분) 동안 노드가 연이은 검사에서 NotReady 상태를 보고하는 경우
- 일정 시간 임계값(약 10분) 동안 노드가 상태를 전혀 보고하지 않는 경우
- 장시간(약 30분) 동안 노드의 부팅 디스크에 디스크 공간이 부족한 경우

# Azure
![Azure Logo](/assets/img/analysis/2021-01-06-18-54-28.png)

MS사의 Azure는 Container를 제공하는 Container Instance 서비스와 Kubernetes를 제공하는 AKS(Azure Kubernetes Service)서비스를 제공하고 있다.

## Azure Container Instance

> 슬로건: 간단한 애플리케이션, 작업 자동화 및 빌드 작업 등 격리된 컨테이너에서 작동할 수 있는 모든 시나리오에 적합한 솔루션

Linux, Windows 컨테이너를 사용할 수 있는 Container Instance 서비스를 제공한다.

내부 구현을 알 수 없지만 '컨테이너에서도 VM처럼 하이퍼바이저 수준으로 격리되도록 보장'한다고 되어 있어 AWS처럼 분리된 VM 위에 컨테이너를 띄울 것으로 추측된다.

단일 컨테이너 인스턴스 기능 외에도, '단일 호스트 컴퓨터에 예약되는', 'Pod과 비슷한', 'Container Group'의 기능을 제공한다. Pod처럼 그룹 내의 컨테이너 사이에 수명 주기, 리소스, 로컬 네트워크 및 저장소 볼륨을 공유할 수 있다.

![Azure Container Group Structure](/assets/img/analysis/2021-01-06-19-07-30.png)

공식 홈페이지에서 위와 같은 Container Group 구성을 확인 할 수 있다. 'VM 위에' Container Group이 구성되어 있는 것을 확인 할 수 있다.

## Azure Kubernetes Service

'Kubernetes를 관리하는 복잡성 및 운영 과부하를 감소'하는 것을 목적으로 Azure에서 '관리되는 Kubernetes 서비스'를 제공한다.

![Azure Kubernetes Service Structure](/assets/img/analysis/2021-01-06-19-12-10.png)

Azure가 상태 모니터링 및 유지 관리 같은 중요 작업을 처리하며, Azure의 VM을 이용해 Node를 구성하게 된다. Azure의 Disk, Network 등의 다양한 기능들을 사용할 수 있다.

Control Plane의 경우 Azure가 관리하고, Worker Node를 사용자가 관리한다.

# TOAST

![TOAST Logo](/assets/img/analysis/2021-01-06-18-51-41.png)

## Kubernetes Service
TOAST의 Compute Instance를 사용하는 Kubernetes 서비스를 제공하고 있다.

Control Plane(Master)을 TOAST에서 관리하며, 고가용성을 보장한다.

Worker Node들은 Node Group으로 관리되며, Kubernetes의 Cluster AutoScaler 기반으로 Node Group들을 Auto Scaling 할 수 있다.


# Naver Cloud (구, NBP)

![Naver Cloud Logo](/assets/img/analysis/2021-01-06-18-38-10.png)

## Kubernetes Service
Naver가 제공하는 'Server' Instance를 이용해 Kubernetes 환경을 구축해 주는 서비스를 제공한다.

2021-01-06 날짜 기준 Classic Platform을 사용하면 Kubernetes v1.17.11 버전을, VPC Platform을 사용하면 Kubernetes v1.16.14와 v1.17.11 중에서 선택이 가능하다.

![Naver Cloud Kubernetes Service Create](/assets/img/analysis/2021-01-06-18-43-56.png)

Kubernetes의 'Cluster AutoScaler'를 사용해 클러스터 자동 확장이 가능하며, Control Plane의 가용성 보장 측면에 관해서는 정보를 찾을 수 없었다. 


# Source
- https://AWS.amazon.com/
- https://cloud.google.com/
- https://docs.microsoft.com/ko-kr/azure/container-instances/container-instances-overview
- https://docs.microsoft.com/ko-kr/azure/container-instances/container-instances-container-groups
- https://docs.microsoft.com/ko-kr/azure/?product=containers
- https://docs.microsoft.com/ko-kr/azure/aks/intro-kubernetes
- https://docs.microsoft.com/ko-kr/azure/aks/concepts-clusters-workloads
- https://www.toast.com/kr/service/container/kubernetes
- https://www.ncloud.com/product/compute/kubernetes
- https://docs.ncloud.com/ko/nks/nks-1-1.html