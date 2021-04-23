---
title:  "[Programmers] 멀쩡한 사각형 (62048번) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers]

classes: wide
toc: false
toc_sticky: true
 
date: 2021-02-12
last_modified_at: 2021-02-23
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/62048

---

# Solution

``` java
class Solution {    
    public long solution(int w, int h) {
        int gcd = getGcd(w, h);
        
        return (long)w*h - ((w/gcd)+(h/gcd)-1)*(long)gcd;
    }
    
    public int getGcd(int w, int h) {
        if(h == 0) {
            return w;
        }
        
        return getGcd(h, w%h);
    }
}
```

---

# 참고한 글

- https://velog.io/@ajufresh/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%EB%A9%80%EC%A9%A1%ED%95%9C-%EC%82%AC%EA%B0%81%ED%98%95-%EB%AC%B8%EC%A0%9C%ED%92%80%EC%9D%B4-Java
- https://mainia.tistory.com/2000