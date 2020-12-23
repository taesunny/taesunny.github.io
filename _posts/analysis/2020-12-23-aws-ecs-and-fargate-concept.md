---
title:  "AWS ECS and Fargate Concept"
excerpt: "About AWS ECS and Fargate"

categories:
  - analysis
tags:
  - [analysis, aws, container, amazon, cloud]

toc: true
toc_sticky: true
 
date: 2020-12-23
last_modified_at: 2020-12-23
---

# AWS ECS and Fargate Concept
![aws logo](/assets/img/analysis/2020-12-23-14-35-23.png)
## AWS ECS
- Amazon Elastic Container Service의 약자
- 완전관리형 컨테이너 오케스트레이션 서비스
- AWS Fargate(컨테이너를 위한 서버리스 컴퓨팅)을 이용해 ECS Cluster 생성이 가능
- 사용 사례
    - 기계 학습, 배치 처리, 웹 어플리케이션

### Task
Kubernetes의 Pod처럼, 사용할 컨테이너들의 그룹 정의를 `Task`를 통해 정의한다. cpu, memory등의 리소스 사용량 및 컨테이너들의 이름, 이미지 등을 설정하며, `requiresCompatibilities`라는 필드로, 사용할 cluster type(`FARGATE` or `EC2`을 정의 할 수 있다.
> [Task 정의 문서 링크](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html)

## AWS Fargate
- 2017년도 출시
- 컨테이너에 적합한 서버리스 컴퓨팅 엔진
- Amazon Elastic Container Service(ECS) 및 Amazon Elastic Kubernetes Service(EKS)에서 동작 가능

## ECS with Fargate
ECS의 Cluster Type은 두가지로 나뉜다.
1. EC2 Instance를 이용한 Cluster Type
2. Fargate가 제공하는 Serverless Infrastructure를 이용한 Cluster Type

### EC2 Instance를 이용한 Cluster Type
Container Instance(==EC2 Instance)들에 `Container Agent`가 실행되고, 컨테이너들을 생성/관리하며, task들과 자원 사용률에 대한 정보를 ECS에 전송한다.

### Fargate가 제공하는 Serverless Infrastructure를 이용한 Cluster Type
EC2 Instance들을 사용하지 않고, 서버리스 방식으로 Fargate를 이용하여 컨테이너들을 생성할 클러스터를 구성할 수 있다.

# 출처
- https://aws.amazon.com/ko/ecs/
- https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html
- https://aws.amazon.com/ko/fargate/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc&fargate-blogs.sort-by=item.additionalFields.createdDate&fargate-blogs.sort-order=desc

