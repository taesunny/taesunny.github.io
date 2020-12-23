---
title:  "Firecracker Concept"
excerpt: "About Firecracker"

categories:
  - analysis
tags:
  - [analysis, opensource, vm, microvm, aws, amazon, cloud]

toc: true
toc_sticky: true

comments: true
 
date: 2020-12-23
last_modified_at: 2020-12-23
---

# Firecracker Concept

![firecracker logo](/assets/img/analysis/2020-12-23-15-05-13.png)

- 오픈 소스, Apache 2.0
- VMM (Virtual Machine Manager)
- KVM을 기반으로 하는 가상 머신 매니저(VMM)를 구현
- microVM을 생성 및 관리하는 RESTful API 제공
- microVM, Linux KVM을 사용하는 새로운 가상화 기술
- Rust로 작성됨

## 특징
- 보안이 뛰어남
- 약 125밀리초 안에 microVM을 시작 가능 (2018년도 기준)
- AWS Lambda, Fargate 등의 서비스가 Firecracker를 사용 중

# 출처
- https://github.com/firecracker-microvm/firecracker
- https://aws.amazon.com/ko/blogs/korea/firecracker-lightweight-virtualization-for-serverless-computing/
- https://aws.amazon.com/ko/about-aws/whats-new/2018/11/firecracker-lightweight-virtualization-for-serverless-computing/


