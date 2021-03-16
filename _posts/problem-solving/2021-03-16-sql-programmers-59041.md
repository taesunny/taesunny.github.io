---
title:  "[Programmers] 동명 동물 수 찾기 (59041번) - MySQL Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [sql, programmers, group-by, order-by, having]

toc: true
toc_sticky: true
 
date: 2021-03-16
last_modified_at: 2021-03-16
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/59041

---

# Solution

``` sql
SELECT NAME, COUNT(NAME) FROM ANIMAL_INS GROUP BY NAME HAVING COUNT(NAME) > 1 ORDER BY NAME
```
