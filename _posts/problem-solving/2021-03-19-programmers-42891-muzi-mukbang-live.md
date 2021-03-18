---
title:  "[Programmers] 무지의 먹방 라이브 (42891번, kakao blind recruitment 2019) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, kakao]

toc: true
toc_sticky: true
 
date: 2021-03-19
last_modified_at: 2021-03-19
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/42891

---

# Solution

``` java
import java.util.*;

class Solution {
    public int solution(int[] food_times, long k) {
        List<int[]> data = new ArrayList<>();

        for (int i = 0; i < food_times.length; i++) {
            data.add(new int[] { i + 1, food_times[i] });
        }

        data.sort((a, b) -> a[1] - b[1]);

        int previousLeftFood = 0;

        for (int i = 0; i < data.size(); i++) {
            int curLeftFood = data.get(i)[1];
            long diff = curLeftFood - previousLeftFood;

            if (diff == 0) {
                continue;
            }

            previousLeftFood = curLeftFood;

            long toEat = diff * (data.size() - i);
            long nextK = k - toEat;

            if (nextK < 0) {
                k %= (data.size()-i);
                data.subList(i, data.size()).sort((a, b) -> a[0] - b[0]);
                return data.get(i + (int) k)[0];
            }

            k = nextK;
        }

        return -1;
    }
}
```

---

# 참고한 글

- https://www.youtube.com/watch?v=4MWxAt4fx5I