---
title:  "Concurrency vs Parallelism (동시성과 병렬성 차이 비교)"
excerpt: "About Concurrency and Parallelism"

categories:
  - analysis
tags:
  - [concurrency, parallelism]

toc: true
toc_sticky: true
 
date: 2021-01-05
last_modified_at: 2021-01-05
---

# Concurrency - 동시성

- 소프트웨어적 특징, Logical
- 각 코어에서 여러 프로세스들을 번갈아가면서 실행하는 경우 (한 시점에 하나의 프로세스 수행)
- ex) 여러 프로세스 각각의 time-sharing slice만큼 하나의 코어에서 수행, 실행 중인 프로세스가 완료 안된 상태로 time slice 끝나면 pause, 다른 프로세스 시작 또는 resume


# Parallelism - 병렬성

- 하드웨어적 특징, Physical
- 한 시점에 여러개의 코어들에서 여러 프로세스들을 실행하는 경우
- ex) 오래 걸리는 연산을 병렬 프로그래밍 하여, 총 연산 시간을 줄인다.


# Source

- [https://en.wikipedia.org/wiki/Concurrent_computing](https://en.wikipedia.org/wiki/Concurrent_computing)
- [https://en.wikipedia.org/wiki/Parallel_computing](https://en.wikipedia.org/wiki/Parallel_computing)
