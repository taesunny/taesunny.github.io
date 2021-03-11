---
title:  "Raft Algorithm"
excerpt: ""

categories:
  - analysis
tags:
  - [raft, algorithm, consensus-algorithm]

toc: true
toc_sticky: true
 
date: 2021-03-09
last_modified_at: 2021-03-09
---

# Raft

Raft는 'Reliable, Replicated, Redundant and Fault-Tolerant' 라는 이름에서 유래한  'Consensus Algorithm'(합의 알고리즘)이다.

Consensus는 'Fault-Tolerant'한 분산 시스템에서 핵심적인 문제이다. Consensus Algorithm은 여러 서버들에서 값의 변화를 동기화하는 과정을 포함한다. Consensus Algorithm을 이용해 여러 서버들 중 다수의(majority) 서버가 살아 있을 경우 Consensus 유지를 할 수 있다.

## Basic Rules

- 하나의 Term 동안에는 하나의 Leader만 선출 가능하다.
- Leader만 Log에 새로운 Entry를 추가할 수 있다.

## 용어

### States

Raft 알고리즘을 사용하는 클러스터의 서버는 다음의 세가지 중 하나의 State를 가진다. Leader가 Master 역할을 하며, 나머지는 Follower State로 부른다. Leader를 선출하는 과정 중에 Leader 후보 State를 Candidate로 부른다.

- Follower
- Candidate
- Leader

### Term

Leader를 뽑는 시점 부터 다음 Leader를 뽑는 시점 사이를 하나의 Term으로 간주하고, Leader를 새로 뽑을 때마다 Term 카운터를 증가시킨다.

### Entry/Log

Entry(==데이터 변경사항)가 각 서버의 Log에 저장되고 관리된다.

### Election Timeout

Follower가 Candidate가 되기 위해 기다리는 시간이며, 150ms ~ 300ms 사이의 랜덤한 시간을 가진다.

### Heartbeat Timeout

Leader가 Follower들에게 전송하는 'Append Entry' 메세지의 간격. heartbeat 또는 데이터 전송 용도로 사용됨

## Leader Election

### Flow

- 최초 - 초기 모든 서버들이 Follower state로 시작
- Election Timeout이 지난 Follower가 Candidate가 되고, 새로운 Election Term을 시작
- Candidate이 vote 시작
- 다른 서버들에게 vote 요청
- 아직 vote 하고 있지 않는 Follower들이 해당 vote 요청에 응답
- Candidate이 다수(majority)의 vote를 받으면 Leader가 됨
- 이후 Leader는 'Append Entry'라는 Heartbeat 메세지를 Follower들에게 전송
- Follower들은 'Append Entry' 메세지에 응답

## Replicating changes

Heartbeat시 사용했던 Append Entries message를 사용하여 Leader의 변경 사항을 모든 시스템 전체에 replicate하여 관리한다. 

### Flow

- Client가 Leader에게 데이터 변경 작업을 요청
- Leader의 Log에 데이터 변경 내용이 저장
- 다음번 heartbeat 시점에 Follower들에게 데이터를 전송
- 다수(majority)의 follower들이 ack를 응답하면 데이터 Commit
- Client들에게 Commit 완료 알림

## Re-Election Issue

기존 Leader가 Fail일 경우 기존 Leader Election 과정을 거쳐 Re-Election을 진행한다. 동시에 다수의 (예시) 두 서버)가 Candidates가 되는 경우에는 'split vote'가 발생 가능 한 이슈가 있으나, re-election 방식으로 대응 가능하다.

- 상황 - 환경에 총 4개의 서버가 있는 경우

### Issue Flow

- 두 서버가 동시에 Election Vote 시작 (예시)
- 각각 최대 1개의 vote밖에 받지 못한다.
- Election Fail
- 다른 서버가 Candidates가 되고, vote 성공 (Election Term 증가)

## Network partition Situation Flow

- 상황
    - A, B, C, D, E Servers 존재
    - B가 Leader
    - A, B / C, D, E 두 파티션(그룹)으로 분리
    - E가 새로운 파티션(그룹)의 Leader (Election Term 증가)

### 변경 사항 발생 시

- B는 데이터 변경사항 발생 시 majority를 충족시키지 못해서 Log Entry를 commit 하지 못함
- E는 majority를 충족시킬 수 있으므로 Commit 성공

### Network Split 복구 시

- Leader B가 보았을 때 Leader E의 Election Term이 더 높아서 Leader B는 사임
- Server A, B는 uncommitted entry들을 rollback 한 후, new leader의 log 데이터들을 동기화

---

# Source

- [https://raft.github.io/](https://raft.github.io/)
- [http://thesecretlivesofdata.com/raft/](http://thesecretlivesofdata.com/raft/)
- [https://ko.wikipedia.org/wiki/래프트_(컴퓨터_과학)](https://ko.wikipedia.org/wiki/%EB%9E%98%ED%94%84%ED%8A%B8_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))