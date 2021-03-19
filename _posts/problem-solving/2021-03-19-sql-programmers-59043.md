---
title:  "[Programmers][SQL] 있었는데요 없었습니다 (59043번) - MySQL Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [sql, programmers, join, left-join]

toc: true
toc_sticky: true
 
date: 2021-03-19
last_modified_at: 2021-03-19
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/59043

---

# Solution

``` sql
SELECT INS.ANIMAL_ID, INS.NAME
FROM ANIMAL_INS AS INS LEFT JOIN ANIMAL_OUTS AS OUTS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE INS.DATETIME > OUTS.DATETIME
ORDER BY INS.DATETIME
```
