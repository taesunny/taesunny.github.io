---
title:  "[Programmers] 구명보트 - Java Solution"
excerpt: "Programmers 구명보트 Java Solution 입니다."

categories:
  - problem-solving
tags:
  - [algorithm]

toc: true
toc_sticky: true
 
date: 2021-01-13
last_modified_at: 2021-01-13
---
# 문제
- https://programmers.co.kr/learn/courses/30/lessons/42885

# Solution

``` java
import java.util.Arrays;

class Solution {
    public int solution(int[] people, int limit) {
        int answer = 0;
        
        Arrays.sort(people);
        int minValueIndex = 0;
        
        for(int maxValueIndex=people.length-1; maxValueIndex>=minValueIndex; maxValueIndex--) {
            
            if(people[maxValueIndex] + people[minValueIndex] <= limit) {
                minValueIndex++;
            }
            
            answer++;
        }

        return answer;
    }
}
```