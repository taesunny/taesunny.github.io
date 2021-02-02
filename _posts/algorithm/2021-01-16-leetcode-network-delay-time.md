---
title:  "[LeetCode] 743. Network Delay Time - Java Solution"
excerpt: "LeetCode 743. Network Delay Time Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, leetcode]

toc: true
toc_sticky: true
 
date: 2021-01-16
last_modified_at: 2021-01-16
---
# 문제
- https://leetcode.com/problems/network-delay-time/

---

# Solution
Dijkstra 알고리즘을 적용했다.

``` java
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        
        int[][] data = new int[n+1][n+1];
        Map<Integer, List<Integer>> reachableNodes = new HashMap<>();
        
        for(int i=0; i<times.length; i++) {
            data[times[i][0]][times[i][1]] = times[i][2];
            
            if(!reachableNodes.containsKey(times[i][0])) {
                reachableNodes.put(times[i][0], new ArrayList<Integer>());
            }
            
            reachableNodes.get(times[i][0]).add(times[i][1]);
        }
        
        Map<Integer, Integer> dist = new HashMap<>();
        
        // e1, e2 : time, index
        PriorityQueue<int[]> pq = new PriorityQueue<>((e1, e2) -> (e1[0] - e2[0]));
        pq.offer(new int[]{0, k});
        
        while(!pq.isEmpty()) {
            int[] top = pq.poll();
            int time = top[0];
            int node = top[1];
            
            if(dist.containsKey(node)) {
                continue;
            }
            
            dist.put(node, time);
            
            if(reachableNodes.containsKey(node)) {
                for(Integer reachableNode : reachableNodes.get(node)) {
                    if(!dist.containsKey(reachableNode)) {
                        pq.offer(new int[]{time + data[node][reachableNode], reachableNode});
                    }
                }
            }  
        }

        if(dist.size() != n) {
            return -1;
        }
        
        int answer = 0;
        
        for(int e : dist.values()) {
            answer = (answer < e) ? e : answer;
        }
        
        return answer;
    }
}
```

---

# Result
- Runtime: 19 ms, faster than 50.56% of Java online submissions for Network Delay Time.
- Memory Usage: 42.9 MB, less than 52.93% of Java online submissions for Network Delay Time.