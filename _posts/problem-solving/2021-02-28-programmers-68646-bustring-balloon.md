---
title:  "[Programmers] 풍선 터트리기 (68646번) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers]

toc: true
toc_sticky: true
 
date: 2021-02-28
last_modified_at: 2021-02-28
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/68646

---

# Solution - Timeout

2중 for문으로 timeout이 발생한다.

``` java
class Solution {
    public int solution(int[] a) {
        if (a.length < 3) {
            return a.length;
        }

        int answer = 2;
        int leftCount = 0;
        int rightCount = 0;

        for (int i = 1; i < a.length - 1; i++) {
            leftCount = 0;
            rightCount = 0;

            for (int j = 0; j < a.length; j++) {
                if (a[j] < a[i] && i != j) {
                    if (j < i) {
                        leftCount++;
                    } else {
                        rightCount++;
                    }
                }
            }

            if (leftCount == 0 || rightCount == 0) {
                answer++;
            }
        }

        return answer;
    }
}
```

---

# Solution

``` java
class Solution {    
    public int solution(int[] a) {

        if (a.length < 3) {
            return a.length;
        }

        int answer = 2;
        boolean leftSideSmallerElementCheck[] = new boolean[a.length];

        int min = a[0];

        for (int i = 1; i < a.length - 1; i++) {
            if (min < a[i]) {
                leftSideSmallerElementCheck[i] = true;
            } else {
                min = a[i];
            }
        }

        min = a[a.length - 1];

        for (int i = a.length - 2; i > 0; i--) {
            if (min < a[i]) {
                if (leftSideSmallerElementCheck[i] == false) {
                    answer++;
                }
            } else {
                min = a[i];
                answer++;
            }
        }

        return answer;
    }
}
```
