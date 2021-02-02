---
title:  "[LeetCode] 841. Keys and Rooms - Java Solution"
excerpt: "LeetCode 841. Keys and Rooms Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, leetcode]

toc: true
toc_sticky: true
 
date: 2021-01-14
last_modified_at: 2021-01-14
---
# 문제
- https://leetcode.com/problems/keys-and-rooms/

---

# Solution

``` java
class Solution {
    
    boolean[] visited;
    
    private void dfs(int roomIndexToEnter, List<List<Integer>> rooms) {
        if(visited[roomIndexToEnter]) {
            return ;
        }
        
        visited[roomIndexToEnter] = true;
        
        for(int i=0; i<rooms.get(roomIndexToEnter).size(); i++) {
             dfs(rooms.get(roomIndexToEnter).get(i), rooms);
        }
    }
    
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        
        int roomNumber = rooms.size();
        
        this.visited = new boolean[roomNumber];
        
        dfs(0, rooms);        
        
        for(int i=0; i<roomNumber; i++) {
            if(!visited[i]) {
                return false;
            }
        }
        
        return true;        
    }
}
```

---

# Result
- Runtime: 0 ms, faster than 100.00% of Java online submissions for Keys and Rooms.
- Memory Usage: 38.8 MB, less than 86.06% of Java online submissions for Keys and Rooms.
