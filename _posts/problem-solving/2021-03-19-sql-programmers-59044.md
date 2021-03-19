---
title:  "[Programmers][SQL] 오랜 기간 보호한 동물(1), (2) (59044번) - MySQL Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [sql, programmers, join, left-join, limit]

toc: true
toc_sticky: true
 
date: 2021-03-19
last_modified_at: 2021-03-19
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/59044
- https://programmers.co.kr/learn/courses/30/lessons/59411

---

# Solution - 오랜 기간 보호한 동물(1)

``` sql
SELECT INS.NAME, INS.DATETIME
FROM ANIMAL_INS AS INS LEFT JOIN ANIMAL_OUTS AS OUTS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE OUTS.DATETIME IS NULL
ORDER BY INS.DATETIME
LIMIT 3
```

---
# Solution - 오랜 기간 보호한 동물(2)

``` sql
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_OUTS AS OUTS, ANIMAL_INS AS INS
WHERE OUTS.ANIMAL_ID = INS.ANIMAL_ID
ORDER BY OUTS.DATETIME - INS.DATETIME
DESC
LIMIT 2
```
