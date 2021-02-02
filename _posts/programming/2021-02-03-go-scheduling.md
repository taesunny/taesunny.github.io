---
title:  "[Go] Go Scheduling Basic"
excerpt: ""

categories:
  - programming
tags:
  - [go, scheduling]

toc: true
toc_sticky: true
 
date: 2021-02-03
last_modified_at: 2021-02-03
---

<br>

# Go Scheduling

## Scheduling Concepts

{:refdef: style="text-align: center;"}
![Go Logo](/assets/img/programming/2021-02-03-00-46-22.png)
{: refdef}

### P

- Logical Processor
- Host의 virtual core 마다 P가 할당
- hyper-threading된 hardware thread가 있을 경우도 각각을 virtual core로 인식

### M

- Machine의 약자
- OS에 의해 관리 되는 OS Thread를 의미
- P가 M에 할당됨

### G

- Goroutine
- Go program 실행단위
- Go 에서의 Application Level Thread 개념
- Context-switching 됨

### GRQ

- Global Run Queue
- 대기중인 Goroutine들이 대기
- GRQ의 Goroutine들이 LRQ로 이동

### LRQ

- Local Run Queue
- P 마다 하나의 LRQ 가 할당됨
- Goroutine 들을 관리

## Go Scheduler

- OS scheduler == preemptive scheduler, unpredictable
- Go Scheduler는 Go runtime의 일부
- Go runtime은 Application에 포함되어 있다. → User Space에서 동작!!!
- preemptive scheduler가 아님 (아직은), Cooperating Scheduler

## Goroutine States

### Waiting

- 무언가를 기다리기 위해 멈춘 상태
- 예시) OS의 System Call 대기, Sync Call (atomic, mutex operation)

### Runnable

- M 에서 실행 될 수 있는 상태

### Executing

- M 에서 실행중인 상태

## Scheduling Decision Triggers

### Go 키워드 사용

- go 키워드에 의해 Goroutine 생성
- Goroutine이 생성 되면

### Garbage Collection

- GC를 위한 Goruoutine도 M에서 실행되어야함

### System Calls

- Goroutine에서 system call 시 M에서 기다리거나 가능한 경우 context-switching

### Synchronization and Orchestration

- ex) atomic, mutex, channel operation

## More Concepts

### GOMAXPROCS

- Goroutine들을 동시에 실행할 최대 OS CPU(thread) 수를 설정

---

# To Research

- The version 1.14 - a new technique of asynchronous preemption
- queuing

---

# Source

- [https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part2.html](https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part2.html)
- [https://golang.org/pkg/runtime/](https://golang.org/pkg/runtime/)