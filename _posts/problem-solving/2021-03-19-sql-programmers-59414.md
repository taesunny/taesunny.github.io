---
title:  "[Programmers][SQL] DATETIME에서 DATE로 형 변환 (59414번) - MySQL Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [sql, programmers, date-format]

toc: true
toc_sticky: true
 
date: 2021-03-19
last_modified_at: 2021-03-19
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/59414

---

# Solution

``` sql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d')
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```