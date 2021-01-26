---
title:  "How to get Docker Container's STDOUT/STDERR using docker logs command"
excerpt: ""

categories:
  - howto
tags:
  - [howto, docker, log]

toc: true
toc_sticky: true
 
date: 2020-07-29
last_modified_at: 2020-07-29
---

{:refdef: style="text-align: center;"}
![docker logo](/assets/img/howto/2021-01-26-20-05-58.png)
{: refdef}


# Intro

바이너리를 직접 실행하지 않고 docker로 실행 할 때, STDOUT을 어떻게 확인할지 고민일 때가 있다. 이때 docker logs 명령을 이용하면 쉽게 확인이 가능하다.

---

# How to

## 기본 사용법

```
docker logs [OPTIONS] CONTAINER
```

## 계속 streaming해서 보기 (following)

계속 streaming해서 보기위해 --follow or -f 옵션을 사용할 수 있다.

```
docker logs --follow CONTAINER
```

## 끝의 원하는 줄 수 만큼 출력

끝에서 원하는 줄 수 만큼 출력할 경우 --tail 옵션을 사용한다.

```
docker logs --tail {줄 수} CONTAINER
docker logs --tail={줄 수} CONTAINER
```

## Timestamp와 함께 출력

Timestamp와 함께 보려면 --timestamps or -t 옵션을 사용한다.

```
docker logs --timestamps CONTAINER
```

## 특정시간 이후의 정보를 출력

특정 시간 --since 옵션을 사용한다.

```
docker logs --since {unix time} CONTAINER
```

---

# Source
- https://docs.docker.com/engine/reference/commandline/logs/