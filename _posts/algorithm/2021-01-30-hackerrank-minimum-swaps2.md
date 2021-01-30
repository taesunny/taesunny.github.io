---
title:  "[HackerRank] Minimum Swaps2 - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, hackerrank, arrays]

toc: true
toc_sticky: true
 
date: 2021-01-30
last_modified_at: 2020-01-30
---
# 문제
- https://www.hackerrank.com/challenges/minimum-swaps-2/problem

---

# Solution

``` java
public class Solution {

    static int minimumSwaps(int[] arr) {
        int answer = 0;

        for(int i=0; i<arr.length; i++) {
            int targetNumber = i+1;
            
            for(int j=i; j<arr.length; j++) {
                if(arr[j] == targetNumber) {
                    if(i != j) {
                        int temp = arr[i];
                        arr[i] = arr[j];
                        arr[j] = temp;
                        answer++;
                    }
                    
                    break;
                }
            }
        }
        
        return answer;
    }

    // skipped code
}
```