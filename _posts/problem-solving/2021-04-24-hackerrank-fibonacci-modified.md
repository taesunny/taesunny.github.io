---
title:  "[HackerRank] Fibonacci Modified - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, hackerrank, fibonacci, biginteger]

toc: true
toc_sticky: true
 
date: 2021-04-24
last_modified_at: 2021-04-24
---
# 문제
- https://www.hackerrank.com/challenges/fibonacci-modified/problem

---

# Solution

초기에 문제의 fibonacciModified method의 return type이 int 형으로 되어 있지만, 테스트 케이스에 int, long type 이상의 결과를 계산해야하므로 java의 BigInteger를 사용한다.

``` java
class Result {
    public static BigInteger fibonacciModified(int t1, int t2, int n) {
        
        BigInteger num1 = new BigInteger(Integer.toString(t1));
        BigInteger num2 = new BigInteger(Integer.toString(t2));
        
        for(int i=3; i<n+1; i++) {
            BigInteger newNum2 = num1.add(num2.multiply(num2));
            num1 = num2;
            num2 = newNum2;
        }
        
        return num2;
    }

}
```
