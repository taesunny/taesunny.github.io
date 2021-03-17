---
title:  "[Programmers][SQL] 입양 시각 구하기(2) (59413번) - MySQL Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [sql, programmers, having, variable]

toc: true
toc_sticky: true
 
date: 2021-03-17
last_modified_at: 2021-03-17
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/59413

---

# Solution

``` sql
SET @hour := -1;

SELECT (@hour := @hour +1) AS 'HOUR',
(SELECT COUNT(*) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @hour) AS COUNT
FROM ANIMAL_OUTS WHERE @hour < 23;
```
