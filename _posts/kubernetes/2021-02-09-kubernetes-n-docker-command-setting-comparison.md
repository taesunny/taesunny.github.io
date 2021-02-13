---
title:  "Kubernetes Pod, Docker Container 커맨드 설정 정리 및 비교 (Pod command, args & Docker Image ENTRYPOINT, CMD)"
excerpt: ""

categories:
  - kubernetes
tags:
  - [kubernetes, docker, command]

toc: true
toc_sticky: true
 
date: 2021-02-09
last_modified_at: 2021-02-13
---

# Intro

Pod에 커맨드 정보 command, args를 정의할 때 Docker image의 Entrypoint와 CMD를 덮어쓰는 기준이 달라 주의해야한다.

---

# 비교

## Kubernetes Pod 커맨드 설정 필드

### command

- define a command
- 배열
- 명시할 경우 Docker Image의 ENTRYPOINT 필드를 덮어쓴다.

### arg

- define arguments for the command
- 배열
- 명시할 경우 명시할 경우 Docker Image의 ENTRYPOINT 필드를 덮어쓴다.

## Docker Image 커맨드 설정 필드

### ENTRYPOINT

- Default Command to Execute at Runtime
- 배열
- 두번째 인자부터 인자로 실행
- docker run 명령에서 --entrypoint 명령으로 덮어 쓸 수 있다.

### CMD

- Default Command or Options
- 배열
- 두번째 인자부터 인자로 실행

### 참고) docker run 사용법

```bash
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

### ENTRYPOINT와 CMD의 차이

- docker run 시 COMMAND를 명시할 경우 CMD가 무시된다.
- CMD와 ENTRYPOINT를 함께 명시할 경우 CMD 혹은 docker run의 COMMAND가 ENTRYPOINT의 인자로 추가된다.

## Kubernetes Pod, Docker Image 커맨드 설정 필드 관계

### Pod의 command 필드

- Docker Image의 ENTRYPOINT 필드를 덮어쓴다.

### Pod의 args 필드

- Docker Image의 CMD 필드를 덮어쓴다.

---

# Source

- [https://docs.docker.com/engine/reference/run/](https://docs.docker.com/engine/reference/run/)
- [https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#)