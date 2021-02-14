---
title:  "[Programmers] 지형 이동 (62050번) - Java Solution"
excerpt: ""

categories:
  - algorithm
tags:
  - [algorithm, programmers, mst, union-find, comparable, kruskal, comparable]

toc: true
toc_sticky: true
 
date: 2021-02-14
last_modified_at: 2021-02-14
---

# 문제
- https://programmers.co.kr/learn/courses/30/lessons/62050

---

# 기억할 점
- comparable 구현을 통한 구현한 객체의 sort 방법
- MST 구현 - 크루스칼, 프림 알고리즘
- 자주 활용되는 union-find 알고리즘

---

# Solution

``` java
import java.io.IOException;
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Collection;
import java.util.Collections;
import java.util.Deque;
import java.util.List;

class Solution {
    
    class Edge implements Comparable<Edge> {
        int cost;
        int[] edge;

        public Edge(int cost, int[] edge) {
            this.cost = cost;
            this.edge = edge;
        }

        @Override
        public int compareTo(Edge e) {
            return this.cost - e.cost;
        }

        public int getCost() {
            return this.cost;
        }

        public int[] getEdge() {
            return this.edge;
        }
    }
    
    int findParent(int[] parent, int targetGroup) {
        if (targetGroup == parent[targetGroup]) {
            return targetGroup;
        }

        return parent[targetGroup] = findParent(parent, parent[targetGroup]);
    }

    boolean isSameParent(int[] parent, int targetGroup1, int targetGroup2) {
        return findParent(parent, targetGroup1) == findParent(parent, targetGroup2);
    }

    void uniteGroup(int[] parent, int targetGroup1, int targetGroup2) {
        parent[findParent(parent, targetGroup2)] = findParent(parent, targetGroup1);
    }
    
    public int solution(int[][] land, int height) {
        int[] dx = new int[] { 0, 1, 0, -1 };
        int[] dy = new int[] { 1, 0, -1, 0 };
        int[][] group = new int[land.length][land.length];
        int groupNumber = 0;
        Deque<int[]> d = new ArrayDeque<>();

        for (int i = 0; i < land.length; i++) {
            for (int j = 0; j < land.length; j++) {

                groupNumber++;

                if (group[i][j] > 0) {
                    continue;
                }

                d.add(new int[] { i, j });
                group[i][j] = groupNumber;

                while (!d.isEmpty()) {
                    int[] target = d.pop();

                    for (int k = 0; k < 4; k++) {
                        int temp_x = target[0] + dx[k];
                        int temp_y = target[1] + dy[k];

                        if (temp_x < 0 || temp_x >= land.length || temp_y < 0 || temp_y >= land.length) {
                            continue;
                        }

                        if (group[temp_x][temp_y] > 0) {
                            continue;
                        }

                        if (Math.abs(land[target[0]][target[1]] - land[temp_x][temp_y]) > height) {
                            continue;
                        }

                        group[temp_x][temp_y] = groupNumber;
                        d.add(new int[] { temp_x, temp_y });
                    }
                }
            }
        }

        List<Edge> e = new ArrayList();

        for (int i = 0; i < land.length; i++) {
            for (int j = 0; j < land.length; j++) {
                for (int k = 0; k < 4; k++) {
                    int temp_x = i + dx[k];
                    int temp_y = j + dy[k];

                    if (temp_x < 0 || temp_x >= land.length || temp_y < 0 || temp_y >= land.length) {
                        continue;
                    }

                    if (group[temp_x][temp_y] == group[i][j]) {
                        continue;
                    }

                    e.add(new Edge(Math.abs(land[i][j] - land[temp_x][temp_y]),
                            new int[] { group[i][j], group[temp_x][temp_y] }));
                }
            }
        }

        int answer = 0;

        int[] parent = new int[groupNumber + 1];
        Collections.sort(e);

        for (int i = 1; i < groupNumber + 1; i++) {
            parent[i] = i;
        }

        for (int i = 0; i < e.size(); i++) {
            int cost = e.get(i).getCost();
            int fromGroup = e.get(i).getEdge()[0];
            int toGroup = e.get(i).getEdge()[1];

            if (!isSameParent(parent, fromGroup, toGroup)) {
                uniteGroup(parent, fromGroup, toGroup);
                answer += cost;
            }
        }

        return answer;
    }
}
```

---

# 참고한 글
- https://yabmoons.tistory.com/470