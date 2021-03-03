---
title:  "[LeetCode] 787. Cheapest Flights Within K Stops - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, leetcode, dijkstra]

toc: true
toc_sticky: true
 
date: 2021-03-03
last_modified_at: 2021-03-03
---

# 문제
- https://leetcode.com/problems/cheapest-flights-within-k-stops/

---

# Solution
```java
class Solution {
    public static int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        int[][] data = new int[n][n];
        Map<Integer, List<Integer>> reachableCities = new HashMap<>();
        int[] kCounts = new int[n];

        for (int i = 0; i < flights.length; i++) {
            data[flights[i][0]][flights[i][1]] = flights[i][2];

            if (!reachableCities.containsKey(flights[i][0])) {
                reachableCities.put(flights[i][0], new ArrayList<>());
            }

            reachableCities.get(flights[i][0]).add(flights[i][1]);
        }

        if (reachableCities.get(src) == null) {
            return -1;
        }

        for (Integer initReachableCity : reachableCities.get(src)) {
            kCounts[initReachableCity] = 1;
        }

        // [0]: dist, [1]: city, [2]: kCount
        PriorityQueue<int[]> pq = new PriorityQueue<>((e1, e2) -> (e1[0] - e2[0]));
        pq.offer(new int[] { 0, src, 0 });

        while (!pq.isEmpty()) {
            int[] top = pq.poll();
            int dist = top[0];
            int city = top[1];
            int kCount = top[2];

            if (kCount > K + 1) {
                continue;
            }

            if (city == dst) {
                return dist;
            }

            if (reachableCities.containsKey(city)) {
                for (Integer reachableCity : reachableCities.get(city)) {
                    int newDist = dist + data[city][reachableCity];
                    int newKCount = kCount + 1;

                    pq.add(new int[] { newDist, reachableCity, newKCount });
                }
            }
        }

        return -1;
    }
}
```

---

# Result

- Runtime: 13 ms, faster than 42.26% of Java online submissions for Cheapest Flights Within K Stops.
- Memory Usage: 41.5 MB, less than 23.35% of Java online submissions for Cheapest Flights Within K Stops.