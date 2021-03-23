---
title:  "[Programmers] 합승 택시 요금 (72413번, kakao blind recruitment 2021) - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, programmers, kakao, floyd-warshall]

toc: true
toc_sticky: true
 
date: 2021-03-23
last_modified_at: 2021-03-23
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/72413

---

# Solution

``` java
class Solution {
    public int solution(int n, int s, int a, int b, int[][] fares) {
        final int MAX_VALUE = 1_000_000_000;

        int[][] dist = new int[n + 1][n + 1];

        for (int i = 0; i < n + 1; i++) {
            for (int j = 0; j < i + 1; j++) {
                if (i == j) {
                    dist[i][j] = 0;
                    continue;
                }

                dist[i][j] = MAX_VALUE;
                dist[j][i] = MAX_VALUE;
            }
        }

        for (int i = 0; i < fares.length; i++) {
            int from = fares[i][0];
            int to = fares[i][1];
            int cost = fares[i][2];

            dist[from][to] = cost;
            dist[to][from] = cost;
        }

        for (int mid = 1; mid < n + 1; mid++) {
            for (int from = 1; from < n + 1; from++) {
                for (int to = 1; to < n + 1; to++) {
                    if (dist[from][mid] == MAX_VALUE || dist[mid][to] == MAX_VALUE) {
                        continue;
                    }

                    dist[from][to] = Math.min(dist[from][to], dist[from][mid] + dist[mid][to]);
                }
            }
        }

        int min = Math.min(dist[s][a] + dist[a][b], dist[s][b] + dist[b][a]);

        for (int i = 1; i < n + 1; i++) {
            if (i == a || i == b) {
                continue;
            }

            if (dist[s][i] == MAX_VALUE || dist[i][a] == MAX_VALUE || dist[i][b] == MAX_VALUE) {
                continue;
            }

            int minTemp = dist[s][i] + dist[i][a] + dist[i][b];
            min = (min > minTemp) ? minTemp : min;
        }

        return min;
    }
}
```
