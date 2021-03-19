---
title:  "[Programmers][SQL] 상위 n개 레코드 (59405번) - MySQL, Oracle Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [sql, programmers, rownum, limit]

toc: true
toc_sticky: true
 
date: 2021-03-19
last_modified_at: 2021-03-19
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/59405

---

# Solution - MySQL

- LIMIT a : 'a' 번째 row 까지 select
- LIMIT a, b : 'a' 번째 row 부터 'b'개의 row들를 select

``` sql
SELECT NAME FROM animal_ins ORDER BY datetime LIMIT 1;
```

# Solution - Oracle

rownum은 1부터 시작한다.

``` sql
select * from (SELECT name from animal_ins order by datetime) where rownum <= 1
```
