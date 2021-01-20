---
title:  "[HackerRank] Beautiful Days at the Movies - Java Solution"
excerpt: "HackerRank Beautiful Days at the Movies Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, hackerrank, implementation]

toc: true
toc_sticky: true
 
date: 2021-01-20
last_modified_at: 2020-01-20
---
# 문제
- https://www.hackerrank.com/challenges/beautiful-days-at-the-movies/problem

---

# Solution
``` java
public class Solution {
    static int getReverse(int i) {
        
        int reverse = 0;
        
        String iString = Integer.toString(i);
        
        int lengthToCut = 0;
        
        for(int j=iString.length()-1; j>=0; j--) {
            if(iString.charAt(j) == '0') {
                lengthToCut++;
            } else {
                break;
            }
        }
        
        iString.substring(iString.length()-lengthToCut);
        
        int by = 1;
        
        for(int j = 0; j<iString.length(); j++) {
            reverse += by*(iString.charAt(j)-'0');
            by *= 10;
        }

        return reverse;
    }
    
    // Complete the beautifulDays function below.
    static int beautifulDays(int i, int j, int k) {
        int answer = 0;
        
        for(int a = i; a<j+1; a++) {
            long temp = a - getReverse(a);
            temp = (temp >= 0) ? temp : -temp;
            
            if(temp % k == 0) {
                answer++;
            }
        }

        return answer;
    }

    ... 생략
}
```