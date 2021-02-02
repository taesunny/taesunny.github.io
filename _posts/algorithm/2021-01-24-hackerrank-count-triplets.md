---
title:  "[HackerRank] Count Triplets - Java Solution"
excerpt: "HackerRank Count Triplets Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, hackerrank, dictionary, hashmap]

toc: true
toc_sticky: true
 
date: 2021-01-24
last_modified_at: 2021-01-24
---
# 문제
- https://www.hackerrank.com/challenges/count-triplets-1/problem

---

# Solution

> indices 뜻: index의 복수

- 3중 for문을 돌면서 찾을 경우 timeout이 발생한다.
- 문제의 조건 중 'i < j < k' 를 지키기 위해, arr List를 for문을 돌면서 체크 한다.

``` java
public class Solution {

    // Complete the countTriplets function below.
    static long countTriplets(List<Long> arr, long r) {
        
        if(arr.size() < 3) {
            return 0;
        }
        
        Map<Long, Long> leftSide = new HashMap<>();
        Map<Long, Long> rightSide = new HashMap<>();
        
        for(int i=0; i<arr.size(); i++) {            
            if(rightSide.containsKey(arr.get(i))) {
                rightSide.put(arr.get(i), rightSide.get(arr.get(i)) + 1);
            } else {
                rightSide.put(arr.get(i), 1L);
            }
        }
        
        long answer = 0L;
        
        for(int i=0; i<arr.size(); i++) {
            long midNumber = arr.get(i);
            long midNumberCountOnTheRightSide = rightSide.get(midNumber);
            rightSide.put(midNumber, midNumberCountOnTheRightSide - 1);
            
            if(rightSide.get(midNumber) == 0L) {
                rightSide.remove(midNumber);
            }
            
            if(midNumber % r == 0) {
                long leftNumber = midNumber/r;
                long rightNumber = midNumber*r;
                
                if(leftSide.containsKey(leftNumber) && rightSide.containsKey(rightNumber)) {
                    answer += leftSide.get(leftNumber) * rightSide.get(rightNumber);
                }
            }
            
            if(leftSide.containsKey(midNumber)) {
                leftSide.put(midNumber, leftSide.get(midNumber) + 1);
            } else {
                leftSide.put(midNumber, (long)1);
            }
        }
        
        return answer;
    }

    // skipped code
}
```