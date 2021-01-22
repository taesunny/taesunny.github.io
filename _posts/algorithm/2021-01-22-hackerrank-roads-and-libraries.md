---
title:  "[HackerRank] Roads and Libraries - Java Solution"
excerpt: "HackerRank Roads and Libraries Java Solution"

categories:
  - algorithm
tags:
  - [algorithm, hackerrank, graphs]

toc: true
toc_sticky: true
 
date: 2021-01-22
last_modified_at: 2020-01-22
---
# 문제
- https://www.hackerrank.com/challenges/torque-and-development/problem

---

# Runtime Error Solution
citiesMap 을 array로 저장하는 경우 runtime error가 발생한다. array로 데이터를 낭비하는 습관을 줄이도록 노력해야겠다.

``` java
public class Solution {

    // Complete the roadsAndLibraries function below.
    static long roadsAndLibraries(int n, int c_lib, int c_road, int[][] cities) {
        
        if(cities == null || cities.length == 0) {
            return (long)n*c_lib;
        }
        
        if (c_lib <= c_road) {
            return (long)c_lib*n;
        }
        
        for(int i=1; i<n+1; i++) {
            citiesMap.put(i, new HashSet<Integer>());
        }
        
        boolean[] visited = new boolean[n+1];
        boolean[][] citiesMap = new boolean[n+1][n+1];
        
        for(int i=0; i<cities.length; i++) {
            int from = cities[i][0];
            int to = cities[i][1];
            
            citiesMap[from][to] = true;
            citiesMap[to][from] = true;
        }
        
        long answer = 0;
        
        for(int city=1; city<n+1; city++) {
            if(!visited[city]) {
                visited[city] = true;
                int cityNumberOfTown = 1;
                Queue<Integer> q = new LinkedList<>();
                q.add(city);
                
                while(!q.isEmpty()) {
                    Integer cityFrom = q.poll();

                    for(int i=1; i<n+1; i++) {
                        if(citiesMap[cityFrom][i] && !visited[i]) {
                            q.add(i);
                
                            cityNumberOfTown++;
                            visited[i] = true;
                        }
                    }
                }
                
                answer += (long)(cityNumberOfTown-1)*(long)c_road + (long)c_lib;
            }
        }

        return answer;
    }

    // skipped code
}
```

# Solution
``` java
public class Solution {

    // Complete the roadsAndLibraries function below.
    static long roadsAndLibraries(int n, int c_lib, int c_road, int[][] cities) {
        
        if(cities == null || cities.length == 0) {
            return (long)n*c_lib;
        }
        
        if (c_lib <= c_road) {
            return (long)c_lib*n;
        }
        
        Map<Integer, Set<Integer>> citiesMap = new HashMap<>();
        
        for(int i=1; i<n+1; i++) {
            citiesMap.put(i, new HashSet<Integer>());
        }
        
        boolean[] visited = new boolean[n+1];
        
        for(int i=0; i<cities.length; i++) {
            int from = cities[i][0];
            int to = cities[i][1];

            citiesMap.get(from).add(to);
            citiesMap.get(to).add(from);
        }
        
        long answer = 0;
        
        for(int city=1; city<n+1; city++) {
            if(!visited[city]) {
                visited[city] = true;
                int cityNumberOfTown = 1;
                Queue<Integer> q = new LinkedList<>();
                q.add(city);
                
                while(!q.isEmpty()) {
                    Integer cityFrom = q.poll();
                    
                    Iterator<Integer> it = citiesMap.get(cityFrom).iterator();
                    
                    while(it.hasNext()) {
                        int connectedCity = it.next();
                        
                        if(!visited[connectedCity]) {
                            q.add(connectedCity);
                            cityNumberOfTown++;
                            visited[connectedCity] = true;
                        }
                    }
                }
                
                answer += (long)(cityNumberOfTown-1)*(long)c_road + (long)c_lib;
            }
        }

        return answer;
    }

    // skipped code
}
```