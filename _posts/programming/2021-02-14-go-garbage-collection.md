---
title:  "[Go] Go Garbage Collection"
excerpt: ""

categories:
  - programming
tags:
  - [go, garbage-collection, gc]

toc: true
toc_sticky: true
 
date: 2021-02-14
last_modified_at: 2021-02-14
---

<br>

# Go Garbage Collection

Go의 Garabge Collection에 대해 정리한다.

## Garbage Collection 정의

프로그램이 동적으로 할당했던 메모리 영역 중, 사용되지 않는 영역을 탐지해서 자동으로 해제해 주는 기능이다. 개발자가 수동으로 메모리 해제를 할 필요가 없다는 장점이 있다.

## Go Garbage Collector

Go의 실행파일안에 Garbage Collector가 내장되어 있어 Java의 JVM처럼 가상머신이 없어도 Garbage Collection이 가능하다.

## 특징

### Compaction이 없다.

JVM처럼 힙 내의 객체들을 압축(재배치)하지 않는다. 개발 초기 계획햇지만 개발 기간 문제로인하여 TCMalloc을 이용해 메모리를 할당하는 방식으로 단편화와 할당 속도 문제를 해결했다.

> 객체들을 재배치 할 경우 힙의 free pointer를 바로 사용하여 빠르게 메모리를 동적할당 할 수 있다는 장점이 있다. (Bump Allocation)

> TCMalloc:  (Thread-Caching Malloc) 구글이 만든 힙 메모리 할당자. 멀티 스레드 환경에서 공유되는 메모리 풀을 이용해 사용하면 스레드별 접근 시 발생하는 동기화로 인해 비효율적. TCMalloc에서는 메모리를 쓰레드별 Thread-local cache(32K이하의 작은 크기 메모리 할당용)와 중앙의 Central Heap으로 나누어서 관리. 메모리 크기를 60개의 클래스로 나누어서 관리하기 때문에 단편화도 해결된다.

### Generational GC가 없다.

세대별 GC를 할 경우 생기는 Write Barrier 오버헤드를 문제로 생각하여 객체들을 세대별 GC하지 않는다.

> 세대별 GC 가정: 대부분의 객체는 금방 접근 불가능해 진다. 오래된 세대에서 젊은 세대로의 참조는 적게 발생한다.

> 세대별 GC를 위해서는 Write Barrier(오래된 세대에서 참조하는 젊은 세대 참조를 따로 기록해야하는 과정)를 사용해서 세대 간 포인터를 기록해야하여 GC를 수행하지 않을 때에도, 오버헤드가 발생한다.

> 세대별 GC의 핵심인 수명이 짧은 객체를 Go에서는 되도록 Stack에 할당시켜 효율성을 높이도록 되어 있다. (스택의 메모리 할당과 해제 시 오버헤드가 적으므로)

## 방식

(Go ~v1.15 기준) Tri-Color Algorithm을 사용하는 CMS(Concurrent Mark & Sweep) 방식의 Garbage Collector 방식만을 사용한다.

### Tri-Color Algorithm (삼색 마킹 알고리즘)

객체들을 흰색, 회색, 검은색으로 나누어 GC를 하는 알고리즘이다.

알고리즘을 이용한 GC 단계는 아래와 같다.

- GC 시작시 모든 객체들은 흰색이다.
- App에서 직접 접근 가능한 스택의 객체들(GC Root)을 확인하며 회색으로 표시한다.
- 회색 객체들을 돌면서 검은색으로 표시하고, 참조하고 있는 포인터가 존재 할 경우 가리키는 객체들을 확인하여 흰색인 경우 회색으로 바꾼다. 이러한 과정을 모든 회색 객체가 사라질 때까지 반복한다.
- 남은 흰색 객체들은 unreachable 이므로 메모리를 해제한다.

위의 과정은 Concurrent 하게 수행 되므로 App Thread - Mutator가 사용중인 객체가 메모리 해제 될 수 있다는 문제점이 있다. (Pointing 되지 않아 흰색으로 남아있는 객체가 사용되도록 변경되는 경우)

이를 방지 하기 위해 Go에서는 Write Barrier를 사용한다. Write Barrier는 Heap의 Pointer가 변경 될 때마다  Mutator가 실행하는 작은 function으로, GC가 스캔가능하도록 객체가 흰색인 경우 회색으로 변경해 준다.

## Tuning Parameters

### GOGC

초기 GC 목표 비율을 설정한다. 기본 메모리 대비 얼만큼의 메모리가 더 할당 되면 GC가 발생하는지에 대한 비율이다. 값이 적을 수록 GC가 자주 일어난다.

- Default: GOGC=100
- Disabling GC: GOGC=off 로 설정
- [SetGCPercent function](https://golang.org/pkg/runtime/debug/#SetGCPercent)을 이용해 Runtime중에 변경 가능하다.

---

# Source

- [https://engineering.linecorp.com/ko/blog/go-gc/](https://engineering.linecorp.com/ko/blog/go-gc/)
- [https://ko.wikipedia.org/wiki/쓰레기_수집_(컴퓨터_과학)](https://ko.wikipedia.org/wiki/%EC%93%B0%EB%A0%88%EA%B8%B0_%EC%88%98%EC%A7%91_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))
- [https://golang.org/pkg/runtime/](https://golang.org/pkg/runtime/)
- [https://aidanbae.github.io/video/gogc/](https://aidanbae.github.io/video/gogc/)
- [https://dave.cheney.net/tag/gogc](https://dave.cheney.net/tag/gogc)
- [https://junghyungil.tistory.com/8](https://junghyungil.tistory.com/8)
- [https://blog.golang.org/go15gc](https://blog.golang.org/go15gc)
- [https://perfectacle.github.io/2019/05/11/jvm-gc-advanced/](https://perfectacle.github.io/2019/05/11/jvm-gc-advanced/)

## tcmalloc

- [https://lethean.github.io/2009/04/29/tcmalloc-google-perftools/](https://lethean.github.io/2009/04/29/tcmalloc-google-perftools/)
- [http://goog-perftools.sourceforge.net/doc/tcmalloc.html](http://goog-perftools.sourceforge.net/doc/tcmalloc.html)
- [https://aidanbae.github.io/code/devops/computer/tcmalloc/](https://aidanbae.github.io/code/devops/computer/tcmalloc/)