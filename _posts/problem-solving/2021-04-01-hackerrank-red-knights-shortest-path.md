---
title:  "[HackerRank] Red knight's shortest path - Java Solution"
excerpt: ""

categories:
  - problem-solving
tags:
  - [algorithm, hackerrank, bfs]

toc: true
toc_sticky: true
 
date: 2021-04-01
last_modified_at: 2021-04-01
---
# 문제
- https://www.hackerrank.com/challenges/red-knights-shortest-path/problem

---

# Solution

dfs로 풀면 3개 정도 timeout이 발생한다.

``` java
public class Solution {
    // Complete the printShortestPath function below.
    static void printShortestPath(int n, int i_start, int j_start, int i_end, int j_end) {
        // Print the distance along with the sequence of moves.

        final String[] MOVEMENT_STRING = new String[] { "UL", "UR", "R", "LR", "LL", "L" };
        final List<int[]> MOVEMENT_DIFF = new ArrayList<>();

        // row diff, col diff
        MOVEMENT_DIFF.add(new int[] { -2, -1 });
        MOVEMENT_DIFF.add(new int[] { -2, 1 });
        MOVEMENT_DIFF.add(new int[] { 0, 2 });
        MOVEMENT_DIFF.add(new int[] { 2, 1 });
        MOVEMENT_DIFF.add(new int[] { 2, -1 });
        MOVEMENT_DIFF.add(new int[] { 0, -2 });

        Queue<Movement> q = new LinkedList<>();
        boolean[][] visited = new boolean[n][n];

        q.add(new Movement(i_start, j_start, new ArrayList<>()));
        visited[i_start][j_start] = true;

        while (!q.isEmpty()) {
            for (int i = 0; i < q.size(); i++) {
                Movement movement = q.poll();
                int[] p = movement.p;
                List<String> path = movement.path;

                if (p[0] == i_end && p[1] == j_end) {
                    System.out.println(path.size());

                    for (String str : path) {
                        System.out.print(str + " ");
                    }

                    return;
                }

                for (int j = 0; j < MOVEMENT_DIFF.size(); j++) {
                    int newRow = p[0] + MOVEMENT_DIFF.get(j)[0];
                    int newCol = p[1] + MOVEMENT_DIFF.get(j)[1];

                    if (newRow < 0 || newCol < 0 || newRow > n - 1 || newCol > n - 1) {
                        continue;
                    }

                    if (visited[newRow][newCol]) {
                        continue;
                    }

                    visited[newRow][newCol] = true;
                    List<String> newPath = new ArrayList(path);
                    newPath.add(MOVEMENT_STRING[j]);
                    q.add(new Movement(newRow, newCol, newPath));
                }
            }
        }

        System.out.println("Impossible");

        return;
    }

    static class Movement {
        int[] p;
        List<String> path;

        Movement(int row, int col, List<String> path) {
            this.p = new int[] { row, col };
            this.path = path;
        }
    }

    // skipped code...
}
```