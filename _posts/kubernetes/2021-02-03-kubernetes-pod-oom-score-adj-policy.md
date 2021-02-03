---
title:  "Kubernetes Pod oom-score-adj Policy"
excerpt: ""

categories:
  - kubernetes
tags:
  - [kubernetes, oom-kill, oom-score-adj, priority]

toc: true
toc_sticky: true
 
date: 2021-02-03
last_modified_at: 2021-02-03
---

# Intro

Kubernetes에서 Pod의 oom-score-adj를 설정하는 로직에 대해 정리한다.

> K8S v1.19.4 기준

---

# Pod oom-score-adj Policy

Pod에 해당 값을 설정하는 주최는 kubelet이다.

## Basic Policy

기본적으로 QoS Class 별로 아래와 같은 oom-score-adj 값이 설정된다.

- Best Effort : 1000
- Guaranteed : -998
- Burstable : 999~2

## Critical Pod Policy

Kubelet에서 'Critical' 하다고 생각되는 Pod들의 oom-score-adj 값을 Guaranteed QoS와 동일한 값인 '-998'로 설정한다.

### Critical Pod 분류

다음과 같은 Pod들을 Critical Pod으로 분류한다.

- Static Pod인 경우 (Pod Source가 API Server가 아닌 경우 - "kubernetes.io/config.source" Annotation의 값으로 판별)
- Mirror Pod인 경우 (Node별 File로 생성되는 Static Pod - "kubernetes.io/config.mirror" Annotation으로 판별)
- Pod의 Priority 값이 20억 이상인 경우 (kube-system Pod들의 경우 20억 이상의 Priority Class를 사용)

## 참고) kube-proxy

kube-proxy는 '--oom-score-adj' 옵션을 통해 oom-score-adj 값을 설정할 수 있다. 기본값은 '-999'이다.

---

# Source

- https://kubernetes.io/ko/docs/reference/command-line-tools-reference/kube-proxy/
- https://github.com/kubernetes/kubernetes/tree/release-1.19
- https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/#node-oom-behavior