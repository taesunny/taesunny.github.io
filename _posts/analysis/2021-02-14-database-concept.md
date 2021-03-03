---
title:  "Database 개념 정리"
excerpt: ""

categories:
  - analysis
tags:
  - [database, db]

toc: true
toc_sticky: true

published: false
 
date: 2021-02-14
last_modified_at: 2021-02-14
---

# Database 개념 정리

Database는 데이터의 집합을 의미한다. DBMS(Database Management System)과 같은 DataBase 관리 시스템에 의해 Database 데이터가 관리된다.

## 관계형 데이터베이스

DB를 관계의 집합으로, 관계를 tuple의 집합으로, tuple을 attribute의 집합으로 여기는 모델이다.

### 용어

- row(==tuple): 행. 데이터의 단위
- column(==attribute): 속성
- table
- schema: 테이블을 정의한 것
- key: 특정 행을 유일하게 식별할 수 있게 하는 열의 모음
    - primary key: 기본 키
    - super key: 관계의 모든 행을 유일하게 식별할 수 있도록 하는 속성의 집합
    - candidate key: 후보 키. 슈퍼 키 중 가장 속성의 수가 적은 것
    - foreign key: 외부키, 다른 테이블과 연결 시키기 위해 사용
        - 참조 무결성: 모든 외부 키 값이 참조하는 테이블의 값으로 존재하는 경우 (외부 키 제약 조건을 이용해 참조 무결성을 확보 가능)
- cardinality: 행의 개수
- degree: 차수. 속성의 개수
- view: 데이블을 수정하지 않고 논리적 테이블을 정의
- 무결성 제약 조건: NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY, CHECK(데이터가 만족해야할 조건을 설정)
- Trigger: ex) 새로운 데이터 입력 시 수행해야할 작업을 설정

### SQL

- Structured Query Language: 구조적 질의 언어
- DML(Data Manipulation Language)
    - 데이터 조회, 추가, 삭제, 갱신을 위한 언어
- DDL(Data Definition Language)
    - DB의 논리적 구조를 정의
    - ex) CREATE, ALT, DROP, RENAME, TRUNCATE
- DCL(Data Control Language)
    - 데이터에 대한 접근 권한을 제어하기 위한 언어
    - ex) 사용자별 접근가능한 데이터를 제어
- TCL(Transaction Control Language)
    - ex) COMMIT, ROLLBACK, SAVEPOINT
- join
    - 일반적으로 where 절을 통해 inner join (데카르트 곱, 모든 가능한 조합)
    - inner join
        - 대응되는 키 값이 있는 경우만 포함
        - 예제

        ```sql
        SELECT id, age FROM Human INNER JOIN Score ON Human.id = Score.id;
        ==
        SELECT id, age FROM Human JOIN Score USING (id);
        ```

    - outer join
        - 대응되는 키 값이 없어도 모든 행을 포함
        - 없는 데이터는 NULL로 표시
        - left outer join: 첫 번째 테이블에 있는 모든 행을 유지
        - right outer join: 두 번째 테이블의 모든 행을 유지
        - full outer join: 양쪽 테이블의 모든 행을 유지
        - 예제

        ```sql
        SELECT id, age, score FROM Human LEFT OUTER JOIN Score ON Human.id = Score.id;
        ```

- MAX, MIN, SUM, AVG
    - 예제

    ```sql
    SELECT AVG(score) FROM Score;
    ```

    ```sql
    SELECT name, AVG(score) FROM Human INNER JOIN Score ON Human.id = Score.id GROUP BY name;
    ```

## Transaction

한 트랜잭션에 속하는 작업 중 하나라도 실패하면 트랜잭션 전체가 실패 한 것으로 간주한다.

- 롤백
- 커밋

### ACID

데이터베이스 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 성질을 가리키는 약어

- 원자성(atomicity): 트랜잭션내의 모든 연산이 적용되거나, 모두 적용되지 않아야한다.
- 일관성(consistency): 트랜잭션 시작 전, 종료 후 DB의 상태가 올바르고 일관된 상태가 유지되어야한다. ex) 참조 무결성이 깨지면 안됨
- 고립성(isolation): 한 트랜잭션에서 DB를 변경한 내용은 커밋 전까지 다른 트랜잭션과 고립되어야한다.
- 영속성(durability): 커밋 후 트랜잭션에 의해 변경된 내용은 영구적이어야한다.

## NoSQL

non SQL 또는 non relational의 약자

### Type

- Document DB
    - 데이터를 Json Document 형태로 저장
    - 어떤 모양의 데이터도 저장이 가능
    - ex) Mongo DB
- key-value DB
    - key value 형태로 데이터를 저장
    - ex) 카산드라 DB, ETCD, Amazon DynamoDB
- Graph DB
    - 각 노드 사이 관계를 표현, Column과 Document가 필요 없을 때 사용
    - ex) TAO, Neo4J

## 분산 데이터베이스

데이터가 네트워크 상에서 분산되어 저장된다.

### CAP 정리

모든 분산 네트워크에는 시간 지연이 있고, 연결이 끊어질 수 있음을 인정. 항상 CAP중 둘을 만족 시킨다.

- 일관성(Consistency): 모든 읽기 작업에서 가장 최근에 쓰여진 것을 반환. 예) 다른 노드에서 쓰기 작업을 해도, 어떠한 노드에서 읽으면 쓰기 작업의 내용이 반영되어야한다.
- 접근성(Accessibility): 모든 요청에 대해 응답하지만, 항상 가장 최근에 쓰인 내용이 반영 되지는 않는다.
- 구분성(Partitionability): 시스템을 노드로 구분, 일부 노드가 다운되도, 시슽메 전체는 동작한다.

---

# To Study

- 정규화
- 무결성
- lock

---

# Source

- [https://ko.wikipedia.org/wiki/데이터베이스](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)
- [https://brunch.co.kr/@toughrogrammer/17](https://brunch.co.kr/@toughrogrammer/17)
- [https://ko.wikipedia.org/wiki/ACID](https://ko.wikipedia.org/wiki/ACID)
- [https://velog.io/@gparkkii/DatabaseSQLNoSQL](https://velog.io/@gparkkii/DatabaseSQLNoSQL)
- 도서. 한빛미디어 출판사. 프로그래밍 면접 이렇게 준비한다.