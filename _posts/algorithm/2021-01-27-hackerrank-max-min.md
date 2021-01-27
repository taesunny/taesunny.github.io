---
title:  "[HackerRank] Max Min - Java Solution"
excerpt: "HackerRank Max Min Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, hackerrank, greedy]

toc: true
toc_sticky: true
 
date: 2021-01-27
last_modified_at: 2020-01-27
---
# 문제
- https://www.hackerrank.com/challenges/angry-children/problem

---

# Solution

``` java
public class Solution {

    // Complete the countTriplets function below.
        static int maxMin(int k, int[] arr) {
        
        Arrays.sort(arr);
        
        int answer = Integer.MAX_VALUE;
        
        for(int i=0; i<arr.length-k+1; i++) {
            int min = arr[i];
            int max = arr[i+k-1];
            
            answer = (answer > max-min) ? max-min : answer;
        }

        return answer;
    }

    // skipped code
}
```