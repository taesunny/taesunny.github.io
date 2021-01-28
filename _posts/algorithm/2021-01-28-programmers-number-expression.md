---
title:  "[Programmers] 숫자의 표현 - Java Solution"
excerpt: "Programmers 숫자의 표현 Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, programmers]

toc: true
toc_sticky: true
 
date: 2021-01-28
last_modified_at: 2020-01-28
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/12924

---

# Solution

``` java
class Solution {
    public int solution(int n) {
        int answer = 0;

        for(int i=1; i<n+1; i++) {
            int sum = 0;

            for(int j=i; j<n+1; j++) {
                sum += j;

                if(sum == n) {
                    answer++;
                }

                if(sum >= n) {
                    break;
                }
            }
        }

        return answer;
    }
}
```

# Bad Timeout Solution
아주 타임아웃이 심한 솔루션, 절대로 하면 안되는 방식

``` java
    public int solution(int n) {
        int answer = 0;

        long[] sumThroghN = new long[10000];

        sumThroghN[1] = 1;

        for (int i = 1; i < 10000; i++) {
            sumThroghN[i] = i * (i + 1) / 2;
        }

        for (int i = n+1; i >= 1; i--) {
            for (int j = 0; j < i + 1; j++) {
                long temp = sumThroghN[i] - sumThroghN[j];
                
                if(temp < n) {
                    break;
                }

                if (temp == n) {
                    answer++;
                }
            }
        }

        return answer;
    }
```