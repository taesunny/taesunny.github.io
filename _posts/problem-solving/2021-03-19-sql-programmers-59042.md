---
title:  "[Programmers][SQL] 없어진 기록 찾기 (59042번) - MySQL Solution"
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
- https://programmers.co.kr/learn/courses/30/lessons/59042

---

# Solution

``` sql
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_OUTS AS OUTS LEFT JOIN ANIMAL_INS AS INS ON OUTS.ANIMAL_ID = INS.ANIMAL_ID
WHERE INS.ANIMAL_ID IS NULL
ORDER BY OUTS.ANIMAL_ID
```
