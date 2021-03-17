---
title:  "[Programmers][SQL] 입양 시각 구하기(1) (59412번) - MySQL Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [sql, programmers, group-by, order-by, having, hour]

toc: true
toc_sticky: true
 
date: 2021-03-17
last_modified_at: 2021-03-17
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/59412

---

# Solution

``` sql
SELECT HOUR(DATETIME) AS HOUR, COUNT(DATETIME) AS COUNT FROM ANIMAL_OUTS GROUP BY HOUR(DATETIME) HAVING HOUR >= 9 and HOUR <= 19 ORDER BY HOUR
```
