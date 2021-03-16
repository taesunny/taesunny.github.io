---
title:  "[HackerRank] Minimum Time Required - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, hackerrank, search]

toc: true
toc_sticky: true
 
date: 2021-02-02
last_modified_at: 2020-02-02
---

# 문제
- https://www.hackerrank.com/challenges/minimum-time-required/problem

---

# Solution

``` java
public class Solution {

    // Complete the minTime function below.
    static long minTime(long[] machines, long goal) {

        long minMachine = machines[0];
        long maxMachine = machines[0];

        for(int i=1; i<machines.length; i++) {
            minMachine = (machines[i] < minMachine) ? machines[i] : minMachine;
            maxMachine = (machines[i] > maxMachine) ? machines[i] : maxMachine;
        }

        long leftIndex = goal * minMachine / machines.length;
        long rightIndex = goal * maxMachine / machines.length;
        
        long days = 0;

        while(true) {
            days = (leftIndex + rightIndex) / 2;
            
            long producedNumber = 0;

            for(int i=0; i<machines.length; i++) {
                producedNumber += days / machines[i];
            }

            if(producedNumber == goal) {
                break;
            }

            if(leftIndex == rightIndex) {
                break;
            }

            if(producedNumber > goal) {
                rightIndex = days;
            } else {
                leftIndex = days + 1;
            }
        }

        for(long i=leftIndex; i<days+1; i++) {
            long producedNumber = 0;

            for(int j=0; j<machines.length; j++) {
                producedNumber += i / machines[j];
            }

            if(producedNumber >= goal) {
                return i;
            }
        }

        return 0l;
    }

    // skipped code
}
```