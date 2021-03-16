---
title:  "[LeetCode] 1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, leetcode, floyd-warshall]

toc: true
toc_sticky: true
 
date: 2021-03-01
last_modified_at: 2021-03-01
---

# 문제
- https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/

---

# Solution
```java
class Solution {
    public static int findTheCity(int n, int[][] edges, int distanceThreshold) {
        int[][] data = new int[n][n];
        final int UNREACHABLE = 100_000;

        for (int[] row : data) {
            Arrays.fill(row, UNREACHABLE);
        }

        for (int i = 0; i < n; i++) {
            data[i][i] = 0;
        }

        for (int i = 0; i < edges.length; i++) {
            int from = edges[i][0];
            int to = edges[i][1];
            int distance = edges[i][2];

            data[from][to] = distance;
            data[to][from] = distance;
        }

        for (int mid = 0; mid < n; mid++) {
            for (int from = 0; from < n; from++) {
                for (int to = 0; to < n; to++) {
                    if (data[from][mid] == UNREACHABLE || data[mid][to] == UNREACHABLE) {
                        continue;
                    }

                    data[from][to] = Math.min(data[from][to], data[from][mid] + data[mid][to]);
                }
            }
        }

        int reachableCountMin = Integer.MAX_VALUE;
        int reachableCountMinNumber = 0;

        for (int i = 0; i < n; i++) {
            int reachableCount = 0;

            for (int j = 0; j < n; j++) {
                if (data[i][j] != 0 && data[i][j] != UNREACHABLE && data[i][j] <= distanceThreshold) {
                    reachableCount++;
                }
            }

            if (reachableCountMin >= reachableCount) {
                reachableCountMin = reachableCount;
                reachableCountMinNumber = i;
            }
        }

        return reachableCountMinNumber;
    }
}
```