---
title:  "[Programmers] 고양이와 개는 몇 마리 있을까 (59040번) - MySQL Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [sql, programmers, group-by, order-by]

toc: true
toc_sticky: true
 
date: 2021-03-16
last_modified_at: 2021-03-16
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/59040

---

# Solution

``` sql
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) AS COUNT from ANIMAL_INS GROUP BY ANIMAL_TYPE ORDER BY ANIMAL_TYPE
```
