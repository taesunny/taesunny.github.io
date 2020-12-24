---
title:  "Kubernetes Pod Priority And Priority Class"
excerpt: "About Priority Class."

categories:
  - kubernetes
tags:
  - [kubernetes, priority]

toc: true
toc_sticky: true
 
date: 2020-12-24
last_modified_at: 2020-12-24
---
# Kubernetes Pod Priority And Priority Class

## 개념
Pod들에 Priority를 설정해 Pod별 중요도를 나타낸다. 스케쥴링 시 Priority가 높은 Pod이 낮은 Pod보다 빨리 스케쥴링 될 확률이 높으며 다음과 같은 경우 Priority가 사용 된다.

- Node의 자원이 부족하여 Pod들을 Eviction할 경우
    - Pod들의 CPU, Memory의 Request 설정값 대비 사용률이 동일할 경우, Priority에 의해 Eviction 우선순위가 결정된다.
- 새로운 Pod 스케쥴링 시에, Node 자원이 부족할 경우, 새로운 Pod의 Priority보다 더 낮은 Priority를 가지는 Pod들이 Preempt(선점, Eviction)될 수 있다.

## PriorityClass
priority를 설정하는 object이다. 특정 namespace에 속하지 않으며, value 필드에 pod에 설정할 priority를 추가한다.

![priority class yaml](/assets/img/kubernetes/2020-12-24-14-22-33.png)

### Fields 상세
- metadata.name
    - 유효한 DNS subdomain name 규칙을 따라야한다.
        - 최대 63자
        - 소문자와 영숫자 또는 `-` 만 포함
        - 영숫자로 시작
        - 영숫자로 끝나야함
    - 'system-' prefix를 사용할 수 없음
- value
    - 값이 클수록 priority가 높음
    - any 32-bit integer value smaller than or equal to 1 billion (10억)
    - 10억 이상의 숫자들은 critical system Pod들에 의해 예약 되어 있음
- globalDefault
    - priorityClassName없이 생성된 Pod의 PriorityClass value를 설정
    - 생성 이전에 생성된 Pod들의 Priority들에는 영향이 없음
    - 해당 값이 설정된 priority class는 시스템에 최대 하나만 존재 가능
    - 설정된 priorityClass가 없다면, priorityClassName이 설정되지 않은 Pod의 기본 priority값은 0
- description
    - 임의의 문자열
    - 클러스터 사용자에게 priority class 사용 용도를 알려주기 위해 사용

### Default System PriorityClass
Critical System Pod들을 위해 예약된 Default System Priority Class이다.
또한 kube-system namespace에 Pod을 생성할 경우만 사용이 가능하다.

- system-cluster-critical
    - value : 2,000,000,000
- system-node-critical
    - value : 2,000,001,000

## Pod Priority 설정 방법
1. Priority Class 생성
2. priorityClassName을 Pod 명세에 추가 하여 Pod을 생성한다.
    ![Pod priority class setting yaml](/assets/img/kubernetes/2020-12-24-14-30-35.png)


## 참고
- Priority Class 삭제 시, 삭제된 PriorityClass를 사용하던 기존 Pod들은 그대로 남아 있다.
- Pod 명세에 priorityClassName을 설정하지 않을 경우 `Priority` Admission Controller에 의해 자동으로 '0' 또는 기본값으로 설정된 priority 값이 설정된다.

# Source
- https://kubernetes.io/docs/concepts/configuration/pod-priority-preemption/