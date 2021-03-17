---
title:  "[Programmers] 후보키 (42890번, kakao blind recruitment 2019) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, kakao]

toc: true
toc_sticky: true
 
date: 2021-03-18
last_modified_at: 2021-03-18
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/42890

---

# Solution

``` java
import java.util.*;

class Solution {
    public int solution(String[][] relation) {
        int rowLength = relation.length;
        int colLength = relation[0].length;

        List<Integer> uniqueList = new LinkedList<>();

        for (int i = 1; i < 1 << colLength; i++) {
            if (isUnique(i, relation)) {
                uniqueList.add(i);
            }
        }

        int answer = 0;

        while (!uniqueList.isEmpty()) {
            int uniqueKey = uniqueList.remove(0);
            answer++;

            Iterator<Integer> it = uniqueList.iterator();
            while (it.hasNext()) {
                Integer target = it.next();

                if ((uniqueKey & target) == uniqueKey) {
                    it.remove();
                }
            }
        }

        return answer;
    }

    private static boolean isUnique(int groupData, String[][] relation) {
        int rowLength = relation.length;
        int colLength = relation[0].length;

        for (int i = 0; i < rowLength - 1; i++) {
            for (int j = i + 1; j < rowLength; j++) {
                boolean isSame = true;

                for (int k = 0; k < colLength; k++) {
                    if ((groupData & 1 << k) != 0) {
                        if (!relation[i][k].equals(relation[j][k])) {
                            isSame = false;
                            break;
                        }
                    }
                }

                if (isSame) {
                    return false;
                }
            }
        }

        return true;
    }
}
```
