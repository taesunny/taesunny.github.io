---
title:  "[HackerRank] Fraudulent Activity Notifications - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, hackerrank, sorting, counting-sorting, 계수정렬]

toc: true
toc_sticky: true
 
date: 2021-04-07
last_modified_at: 2021-04-07
---
# 문제
- https://www.hackerrank.com/challenges/fraudulent-activity-notifications/problem

---

# Solution

``` java
public class Solution {
    // Complete the activityNotifications function below.
    static int activityNotifications(int[] expenditure, int d) {
        int[] countData = new int[201];

        for (int i = 0; i < d; i++) {
            countData[expenditure[i]]++;
        }

        int answer = 0;

        for (int i = d; i < expenditure.length; i++) {
            int today = expenditure[i];
            double preAvg = getAverage(countData, d);

            if ((double) today >= preAvg * (double) 2) {
                answer++;
            }

            countData[today]++;
            countData[expenditure[i - d]]--;
        }
        return answer;
    }

    static double getAverage(int[] countData, int d) {
        if (d % 2 == 0) {
            int targetNTh1 = d / 2;

            int cnt = 0;
            for (int i = 0; i < countData.length; i++) {
                cnt += countData[i];

                if (cnt > targetNTh1) {
                    return (double) i;
                } else if (cnt == targetNTh1) {
                    for (int j = i + 1; j < countData.length; j++) {
                        if (countData[j] > 0) {
                            return (double) (i + j) / (double) 2;
                        }
                    }
                }
            }
        } else {
            int targetNTh = d / 2 + 1;

            int cnt = 0;
            for (int i = 0; i < countData.length; i++) {
                cnt += countData[i];
                if (cnt >= targetNTh) {
                    return (double) i;
                }
            }
        }

        return -1;
    }

    // skipped code...
}
```