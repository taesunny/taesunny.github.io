---
title:  "[HackerRank] Sherlock and Cost - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, hackerrank, dp]

toc: true
toc_sticky: true
 
date: 2021-04-05
last_modified_at: 2021-04-05
---
# 문제
- https://www.hackerrank.com/challenges/sherlock-and-cost/problem

---

# Solution

``` java
public class Solution {
    // Complete the cost function below.
    static int cost(int[] B) {
        if (B.length == 1) {
            return B[0];
        }
        
        // index 0: sum with the number chosen, index 1: sum with number 1 chosen
        int[] frontSum = new int[2];
        frontSum[0] = 0;
        frontSum[1] = 0;
        int[] frontSumBackup = new int[2];

        for (int i = 1; i < B.length; i++) {
            frontSumBackup[0] = frontSum[0];
            frontSumBackup[1] = frontSum[1];
            frontSum[0] = Math.max(Math.abs(B[i] - 1) + frontSumBackup[1],
                    Math.abs(B[i] - B[i - 1]) + frontSumBackup[0]);
            frontSum[1] = Math.abs(1 - B[i - 1]) + frontSumBackup[0];
        }

        return Math.max(frontSum[0], frontSum[1]);
    }

    // skipped code...
}
```