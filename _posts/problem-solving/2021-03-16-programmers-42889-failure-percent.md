---
title:  "[Programmers] 실패율 (42889번, kakao blind recruitment 2019) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, kakao]

toc: true
toc_sticky: true
 
date: 2021-03-16
last_modified_at: 2021-03-16
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/42889

---

# Solution

``` java
import java.util.*;

class Solution {
    public int[] solution(int N, int[] stages) {
        Arrays.sort(stages);

        Set<Integer> stagesComputable = new HashSet<>();
        SortedMap<Double, List<Integer>> percentageMap = new TreeMap<>(Comparator.reverseOrder());
        int peopleTried = stages.length;
        int peopleToClear = 1;
        int previousStage = stages[0];

        for (int i = 1; i < stages.length + 1; i++) {
            if (i == stages.length || stages[i] != previousStage) {
                if (previousStage > N) {
                    break;
                }

                double failurePercent = (double) peopleToClear / peopleTried;

                if (!percentageMap.containsKey(failurePercent)) {
                    List<Integer> keys = new ArrayList<>();
                    keys.add(previousStage);
                    percentageMap.put(failurePercent, keys);
                } else {
                    List<Integer> keys = percentageMap.get(failurePercent);
                    keys.add(previousStage);
                    percentageMap.put(failurePercent, keys);
                }

                stagesComputable.add(previousStage);

                peopleTried -= peopleToClear;
                peopleToClear = 0;

                if (i == stages.length) {
                    break;
                }
            }

            previousStage = stages[i];
            peopleToClear++;
        }

        int[] answer = new int[N];

        int i = 0;
        for (Double percent : percentageMap.keySet()) {
            List<Integer> curStages = percentageMap.get(percent);

            for (Integer stage : curStages) {
                answer[i++] = stage;
            }
        }

        for (int j = 1; j <= N && i < N; j++) {
            if (!stagesComputable.contains(j)) {
                answer[i++] = j;
            }
        }

        return answer;
    }
}
```
