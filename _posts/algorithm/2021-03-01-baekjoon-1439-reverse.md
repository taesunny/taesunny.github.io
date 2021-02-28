---
title:  "[백준] 1439. 뒤집기 - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, baekjoon]

toc: true
toc_sticky: true
 
date: 2021-03-01
last_modified_at: 2021-03-01
---

# 문제
- https://www.acmicpc.net/problem/1439

---

# Solution

``` java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String input = sc.next();

        char[] data = input.toCharArray();

        int transitionTo1 = 0;
        int transitionTo0 = 0;

        if (data[0] == '1') {
            transitionTo1++;
        } else {
            transitionTo0++;
        }

        for (int i = 1; i < data.length; i++) {
            if (data[i] != data[i - 1]) {
                if (data[i] == '0') {
                    transitionTo0++;
                } else {
                    transitionTo1++;
                }
            }
        }

        int answer = Math.min(transitionTo1, transitionTo0);
        System.out.println(answer);

        sc.close();
    }
}
```
